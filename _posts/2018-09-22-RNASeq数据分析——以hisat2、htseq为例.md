---
layout: post
title:  "RNASeq数据分析——以hisat2、htseq为例"
categories: 生信
tags: ubuntu hisat2 htseq rnaseq 
author: y570pc
---

* content
{:toc}

## 常见linux命令
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
{% highlight js%}
cd ~/ncbi/public/sra
for ((i=56;i<=57;i=i++));do prefetch -v SRR35899$i ;done
{% endhighlight %}

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

## 参考资料
[1]. [浙大植物学小白的转录组笔记](https://www.wxwenku.com/d/102180058)

[2]. [转录组入门(6)： reads计数](https://www.jianshu.com/p/e9742bbf83b9)












