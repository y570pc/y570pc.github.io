---
layout: post
title:  "RNASeq数据比对——以hisat2、htseq为例"
category: 'bioinformation'
tags: ubuntu hisat2 htseq rnaseq 
author: y570pc
image: "http://blog.genesino.com/images/ngs/RNAseq_complex_workflow.png"
---

* content
{:toc}

## 原理

#### 去接头

Trim galore，是可以自动检测adapter。trimmomatic只是针对Illumina高通量测序平台设计的接头去除和低质量reads清洗软件。Trim Galore是对FastQC和Cutadapt的包装，适用于所有高通量测序，包括RRBS(Reduced Representation Bisulfite-Seq ), Illumina、Nextera 和smallRNA测序平台的双端和单端数据。主要功能包括两步：第一步首先去除低质量碱基，然后去除3' 末端的adapter，如果没有指定具体的adapter，程序会自动检测前1million的序列，然后对比前12-13bp的序列是否符合不同测序平台的adapter类型。

其他的分析流程有：HISAT-StringTie-Ballgown[06]。

## 常见linux命令<sup>[3]</sup>
{% highlight js%}
.   //当前目录
~    //当前用户目录
cd ..   //返回上一级目录
mv  dir1  dir1    //将dir1移动到dir2
mkdir dir1   //创建一个叫做'dir1'的目录' 
rm -f file1   //删除一个叫做'file1'的文件' 
rm -rf dir1  //删除一个叫做'dir1'的目录并同时删除其内容
gunzip file1.gz   //解压一个叫做 'file1.gz'的文件
unzip file1.zip   //解压文件
tar -zxvf archive.tar.gz 解压一个gzip格式的压缩包 
wget  file1  //下载文件
//修改环境变量
vim ~/.bashrc
PATH=$PATH:~/software/sratoolkit.2.9.2-ubuntu64/bin
source ~/.bashrc
//for循环
sum=0
for (( i=1; i<=100; i++ ))
  do  
   sum=$(( $sum + $i ))
  done
echo "1+2+3+...+100=$sum"
{% endhighlight %}

## 分析环境搭建
* [SRA Toolkit](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software)
* [Fastqc](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
* MultiQC
* [HISAT2](http://ccb.jhu.edu/software/hisat2/index.shtml)
* [SAMtools](https://sourceforge.net/projects/samtools/files/samtools/)
* RSeQC，pip install RSeQC
* HTSeq，conda install HTSeq

*RSeQC只支持python2.7，所以需要anaconda的默认python3.5版本切换为python2.7版本*
{% highlight js%}
//ubuntu下anaconda自由切换python2.7与python3.5
//基于 python3.5 创建一个名为py3 的环境
conda create --name py3 python=3.5
//基于 python2.7 创建一个名为py2 的环境
conda create --name py2 python=2.7
//激活相关环境
//激活py2   
source activate py2 
//注销py2    
source deactivate py2
{% endhighlight %}

## 数据下载
#### 下载mRNA-seq数据

数据来自于GSE81916中敲除了AKAP95基因的人293细胞的mRNA-seq数据SRR3589956、SRR3589957。
```
###方法一
cd ~/ncbi/public/sra
for ((i=56;i<=57;i=i++));do prefetch -v SRR35899$i ;done

###方法二
ftp://ftp.ncbi.nlm.nih.gov/sra/sra-instant/reads/ByRun/sra/SRR/SRR548/SRR5483090/
##sra转fasta
#单端：
fastq-dump --fasta SRR6470965.sra    #结果生成 ：SRR6470965.fasta
fastq-dump  SRR6470965.sra     #结果生成 ：SRR6470965.fastq
#双端：
fastq-dump --split-3  DRR002018.sra     #结果生成   DRR002018_1.fastq；DRR002018_2.fastq

```

#### 参考基因组hg19下载

[下载地址](http://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/chromFa.tar.gz)，存放到~/ncbi/public/reference/genome目录下。
{% highlight js%}
cd ~/ncbi/public/reference/genome
tar -zxvf chromFa.tar.gz
cat *.fa > hg19.fa  //将所有的染色体信息整合在一起，重定向写入hg19.fa文件，得到参考基因组
rm -rf chr*
{% endhighlight %}

#### 参考基因组注释gtf文件下载

第28版本的hg19人类基因组注释信息[GTF文件下载地址](ftp://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_28/GRCh37_mapping/gencode.v28lift37.annotation.gtf.gz)，第28版本的hg19人类基因组注释信息[GTF文件下载地址](ftp://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_28/GRCh37_mapping/gencode.v28lift37.annotation.gff3.gz)。
文件存放到~/ncbi/public/reference/annotation
{% highlight js%}
cd ~/ncbi/public/reference/annotation
gunzip *.gz && rm -rf *.gz
{% endhighlight %}

#### 人类基因组index文件下载

[下载地址](ftp://ftp.ccb.jhu.edu/pub/infphilo/hisat2/data/hg19.tar.gz)，存放到~/ncbi/public/reference/index目录下。
{% highlight js%}
cd ~/ncbi/public/reference/index
tar -zxvf *.tar.gz && rm -rf *.tar.gz
{% endhighlight %}

#### 参考基因组注释bed文件下载

[hg19_RefSeq.bed下载地址](https://sourceforge.net/projects/rseqc/files/BED/Human_Homo_sapiens/)，存放到~/ncbi/public/reference/目录下。

## 数据分析
#### fastq-dump将sra数据转换成fastq格式

{% highlight js%}
for ((i=56;i<=57;i=i++));do fastq-dump --gzip --split-3 -A ~/ncbi/public/sra/SRR35899$i.sra -O ~/ncbi/public/fastq ;done
{% endhighlight %}
*--gzip 使得输出的结果是.gz的格式；--split-3 对于PE测序，输出的结果是*_1.fastq.gz和*_2.fastq.gz；-A 输入sra文件的绝对路径。*

#### Fastqc进行测序结果的质控

{% highlight js%}
cd ~/ncbi/public/fastq
fastqc -o ~/ncbi/public/QC *.fastq.gz
{% endhighlight %}

#### MultiQC对质控结果合并

{% highlight js%}
cd ~/ncbi/public/QC
multiqc *fastqc.zip --ignore *.html
{% endhighlight %}

#### 测序数据比对到参考基因组上

{% highlight js%}
cd ~/ncbi/public/
for i in `seq 56 57`
do
    hisat2 -t -x reference/index -1 fastq/SRR35899${i}_1.fastq.gz -2 SRR35899${i}_2.fastq.gz -S aligned/SRR35899${i}.sam 
done
{% endhighlight %}

#### sam格式转换为bam、排序、建立index

{% highlight js%}
cd ~/ncbi/public/aligned
for i in `seq 56 57`
do
    samtools view -S SRR35899${i}.sam -b > SRR35899${i}.bam
    samtools sort SRR35899${i}.bam -o SRR35899${i}_sorted.bam
    samtools index SRR35899${i}_sorted.bam
done
{% endhighlight %}

#### 比对质控

{% highlight js%}
bam_stat.py -i SRR3589956_sorted.bam
{% endhighlight %}
 
#### 计算基因组覆盖率

{% highlight js%}
read_distribution.py -i SRR3589956_sorted.bam -r reference/hg19_RefSeq.bed
{% endhighlight %}

#### resds计数

{% highlight js%}
//单个处理
htseq-count -r pos -f bam SRR3589956_sorted.bam ~/ncbi/public/reference/annotation/gencode.v28lift37.annotation.gtf.gz > SRR3589956.count

//批处理
for i in `seq 56 57`
do
    htseq-count -s no -r pos -f bam aligned/SRR35899${i}_sorted.bam ~/ncbi/public/reference/annotation/gencode.v28lift37.annotation.gtf.gz > RNA-Seq/matrix/SRR35899${i}.count 2> RNA-Seq/matrix/SRR35899${i}.log
done
{% endhighlight %}

#### 合并reads计数数据形成表达矩阵

{% highlight python%}
#方法一
import sys
mydict = {}
for file in sys.argv[1:]:
    for line in open(file, 'r'):
        key,value = line.strip().split('\t')
        if key in mydict:
            mydict[key] = mydict[key] + '\t' + value
        else:
            mydict[key] = value
for key,value in mydict.items():
    print(key + '\t' + value +'\n')

#方法二
paste *.txt | awk '{printf $1 "\t";for(i=2;i<=NF;i+=2) printf $i"\t";printf $i}'
{% endhighlight %}

bedtools makewindows  计算序列覆盖度

## 参考资料
[01]. [浙大植物学小白的转录组笔记](https://www.wxwenku.com/d/102180058)

[02]. [转录组入门(6)： reads计数](https://www.jianshu.com/p/e9742bbf83b9)

[03]. [《Advanced Bash-Scripting Guide》 in Chinese](https://linuxstory.gitbooks.io/advanced-bash-scripting-guide-in-chinese/)

[04]. [RNASEQ学习流程](https://uteric.github.io/RNASEQ%E5%AD%A6%E4%B9%A0%E6%B5%81%E7%A8%8B/)

[05]. [Trim Galore ——自动检测adapter的质控软件](https://www.jianshu.com/p/7a3de6b8e503)

[06]. [RNA-seq数据分析---方法学文章的实战练习](https://www.jianshu.com/p/1f5d13cc47f8)








