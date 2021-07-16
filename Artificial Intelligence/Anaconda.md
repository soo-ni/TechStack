# Anaconda 설치

## Environment

* Ubuntu 18.04.4 LTS (64bit)
* Memory: 32G
* AMD® Ryzen 5 3600x 6-core processor × 12
* GeForce GTX TITAN X

## Install

https://www.anaconda.com/distribution/ 에서 Linux version 다운로드 (2021.04 기준)

```shell
$ bash Anaconda3-2020.11-Linux-x86_64.sh
# license: yes / init : no 차례로 입력
```

```sh
$ gedit ~/.bashrc

# bashrc file에 추가
# anaconda path setting
export PATH=~/anaconda3/bin:~/anaconda3/condabin:$PATH

 $ source ~/.bashrc
 $ conda -V	# conda version check
 $ conda config --set auto_activate_base False
```

## How to

```sh
$ conda create --name 가상환경이름 --clone 복제할가상환경이름 python=원하는버전
```

```sh
$ conda activate 가상환경이름
```





### 참고

* [https://somjang.tistory.com/entry/PythonUbuntu1804-LTS%EC%97%90-Anaconda%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0](https://somjang.tistory.com/entry/PythonUbuntu1804-LTS에-Anaconda설치하기)
* https://m.blog.naver.com/tinz6461/222135158313





