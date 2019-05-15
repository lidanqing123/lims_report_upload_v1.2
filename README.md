# lims_report_upload_v1.2



## 简介
`Lims_report_uploader`是用于将结题报告从天津或南京集群传到lims系统的工具.

## 版本
![v1.2 ](https://raw.githubusercontent.com/lidanqing123/lims_report_upload_v1.2/master/README.md)
   

## 集群路径

天津集群
```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python /TJPROJ1/MICRO/lidanqing/lims/lims_report_upload_v1.2/Lims_report_uploader -h
```
南京集群
```
/NJPROJ2/MICRO/share/software/Anaconda/anaconda3/bin/python  /NJPROJ2/MICRO/PROJ/lidanqing/lims/lims_report_upload_v1.2/Lims_report_uploader -h
```

## 使用方法

* 首先,需要通过`init`命令初始化配置自己的lims账号和密码,此后,使用不需要再重新配置.
* 使用`R`命令来上传结题报告;
* 如果不清楚SOP编号,可通过`search`命令查询可选SOP

```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py -h
usage: Lims_report_uploader.py [-h] [-V] {init,search,R,Q,M} ...

Description:
这个脚本用于将结题报告从集群传到lims系统中.
    1 首先,需要通过’init’命令初始化配置lims账号和密码;
    2 上传结题报告使用’R’命令;
    3 如果不清楚SOP编号,可通过’search’命令查询可选SOP

author: lidanqing@novogene.com
version:  1.1.1

positional arguments:
  {init,search,R,Q,M}  sub-command
    init               开始初始化lims账户信息
    search             查询项目SOP
    R                  上传结题报告
    Q                  上传QC报告
    M                  上传 Mapping 报告

optional arguments:
  -h, --help           show this help message and exit
  -V, --version        show program's version number and exit

Example:
            Lims_report_uploader.py init -h
            Lims_report_uploader.py search -h
            Lims_report_uploader.py R -h
            Lims_report_uploader.py Q -h
            Lims_report_uploader.py M -h
```


### 初始化lims账号信息
初始化lims账号信息直接使用`init`命令
```
$ /PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py init
```
查看帮助信息可加上`-h`参数:
```
$ /PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py init -h 
usage: Lims_report_uploader init [-h]

Description:
    ’init’命令用于初始化配置lims账号和密码等信息.命令行中不用给其他任何参数.

optional arguments:
  -h, --help  show this help message and exit

Example:
    Lims_report_uploader init

```


### SOP编号查询

通过项目分期编号,可以使用`search`命令查询可选的SOP

```
$  /PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py search -s P101SC18072239-01-F002 
	SOP_code	SOP方法
############################################################
	SOPMC00038	ANI分析（每株）
	SOPMC00039	cgMLST
	SOPMC00040	QC
	SOPMC00041	比较基因组分析（SNP／InDel／SV检测及注释）
	SOPMC00042	比较基因组分析（共线性）
	SOPMC00043	标准分析
	SOPMC00044	群体进化分析（core-pan基因分析）
	SOPMC00045	群体进化分析（基因家族）
	SOPMC00046	群体进化分析（进化选择压力分析）
	SOPMC00047	群体进化分析（群体地理传播途径预测）
	SOPMC00048	群体进化分析（群体进化分歧时间预测）
	SOPMC00049	群体进化分析（群体进化树分析）
	SOPMC00050	转座子分析（基于蛋白+核酸序列）
	SOPMC00051	转座子分析（基于蛋白序列）
	SOPMC00052	转座子分析（基于核酸序列）

```
查看帮助信息可使用`-h`参数:
```
$  /PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py search -h
usage: Lims_report_uploader search [-h] -s STAGECODE

Description:
    提供项目分期编号,使用'search'命令,可查询此产品可选SOP.

optional arguments:
  -h, --help            show this help message and exit
  -s STAGECODE, --stage_code STAGECODE
                        分期编号

Example:
    Lims_report_uploader search -s  P101SC18072239-01-F002
    Lims_report_uploader search --stage_code  P101SC18072239-01-F002
```


### 结题报告上传
报告上传是此程序的主要功能. 需要配置的参数相对较多:
查看帮助信息可使用`-h`参数:
```
$/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py R -h
usage: Lims_report_uploader.py R [-h] -i file -s STAGECODE -r {Q,M,R} -l file
                                 -d num --SOP SOP [-m REMARK] [-e EMAIL]

Description:
    'R'是本脚本最核心的功能,用于上传结题报告.

optional arguments:
  -h, --help            show this help message and exit
  -i file, --input file
                        集群中报告文件的路径, 只支持'.zip', '.tar.gz', '.gz',
                        '.rar'等类型的压缩文件
  -s STAGECODE, --stage_code STAGECODE
                        项目分期编号
  -r {Q,M,R}, --report_type {Q,M,R}
                        结题报告类型,例如:QC报告、mapping报告、结题报告
  -l file, --sample_list file
                        样本列表，每行一个样本诺和编号
  -d num, --total_data num
                        总数据量,以G为单位
  --SOP SOP             SOP编号,多个SOP编号的话以英文字符逗号分开,like:"SOPMC00038,SOPMC00039".
                        配置前可以用过'search'命令查询可选SOP.
  -m REMARK, --remark REMARK
                        备注信息,默认为空
  -e EMAIL, --email EMAIL
                        收件人邮箱,用于接收报告上传后的状态信息,可设置多个收件人(用";"号分隔).例如:"lidanqing@n
                        ovogene.com;liuchen@novogene.com".默认为空

Example:
    Lims_report_uploader.py R -i P101SC18072239-01-B1-3.zip -s P101SC18072239-01-F002 -r Q -l samplelist.txt -d 2 --SOP SOPMC00039,SOPMC00040 -m "正常" -e "lidanqing@novogene.com;liuchen@novogene.com"

    Lims_report_uploader.py R --input P101SC18072239-01-B1-3.zip --stage_code P101SC18072239-01-F002 --report_type Q --sample_list samplelist.txt --total_data 2 --SOP SOPMC00039,SOPMC00040 --remark "正常" --email "lidanqing@novogene.com;liuchen@novogene.com"

```

### QC报告上传

```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py Q -h
usage: Lims_report_uploader.py Q [-h] -i file -s STAGECODE [-m REMARK]
                                 [-e EMAIL]

Description:
    'Q'的功能是用于上传QC报告.

optional arguments:
  -h, --help            show this help message and exit
  -i file, --input file
                        集群中QC报告文件的路径, 只支持'.zip', '.tar.gz', '.gz',
                        '.rar'等类型的压缩文件
  -s STAGECODE, --stage_code STAGECODE
                        项目分期编号
  -m REMARK, --remark REMARK
                        备注信息,默认为空
  -e EMAIL, --email EMAIL
                        收件人邮箱,用于接收报告上传后的状态信息,可设置多个收件人(用";"号分隔).例如:"lidanqing@n
                        ovogene.com;liuchen@novogene.com".默认为空

Example:
    Lims_report_uploader.py Q -i P101SC18072239-01-B1-3.zip -s P101SC18072239-01-F002 -m "正常"  -e "lidanqing@novogene.com;liuchen@novogene.com"

    Lims_report_uploader.py Q --input P101SC18072239-01-B1-3.zip --stage_code P101SC18072239-01-F002 --remark "正常" --email "lidanqing@novogene.com;liuchen@novogene.com"
```

### Mapping 报告上传
```
/PUBLIC/software/MICRO/Anaconda/anaconda3/bin/python Lims_report_uploader.py M -h
usage: Lims_report_uploader.py M [-h] -i file -s STAGECODE [-m REMARK]
                                 [-e EMAIL]

Description:
    'M'的功能是用于上传 mapping 报告.

optional arguments:
  -h, --help            show this help message and exit
  -i file, --input file
                        集群中QC报告文件的路径, 只支持'.zip', '.tar.gz', '.gz',
                        '.rar'等类型的压缩文件
  -s STAGECODE, --stage_code STAGECODE
                        项目分期编号
  -m REMARK, --remark REMARK
                        备注信息,默认为空
  -e EMAIL, --email EMAIL
                        收件人邮箱,用于接收报告上传后的状态信息,可设置多个收件人(用";"号分隔).例如:"lidanqing@n
                        ovogene.com;liuchen@novogene.com".默认为空

Example:
    Lims_report_uploader.py M -i P101SC18072239-01-B1-3.zip -s P101SC18072239-01-F002 -m "正常"  -e "lidanqing@novogene.com;liuchen@novogene.com"

    Lims_report_uploader.py M --input P101SC18072239-01-B1-3.zip --stage_code P101SC18072239-01-F002 --remark "正常" --email "lidanqing@novogene.com;liuchen@novogene.com"
```


## FAQ

### 1. UnicodeEncodeError: 'charmap' codec can't encode characters in position 85-100
![](https://raw.githubusercontent.com/lidanqing123/Lims__report_uploader/master/QQ%E5%9B%BE%E7%89%8720181226200832.png)

假如出现这个报错.可能是linux系统的stdout编码不对.可通过如下命令查看.

![](https://raw.githubusercontent.com/lidanqing123/Lims__report_uploader/master/QQ%E5%9B%BE%E7%89%8720181226200951.png)
   
用过`vi`命令将`~/.bashrc`文件加入下面一行信息.把环境变量LANG改为'zh_CN.UTF-8'
```
export LANG='zh_CN.UTF-8'
```






