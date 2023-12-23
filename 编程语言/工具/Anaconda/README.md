
# anaconda 命令

- 查看conda版本 conda --version  
- 更新conda版本 conda update conda  
- 查看都安装了那些依赖库 conda list  
- 创建新的python环境 conda create --name myenv  
- 并且还可以指定python的版本 conda create -n myenv python=3.7  
- 创建新环境并指定包含的库 conda create -n myenv scipy  
- 并且还可以指定库的版本 conda create -n myenv scipy=0.15.0  
- 复制环境 conda create --name myclone --clone myenv  
- 查看是不是复制成功了 conda info --envs  
- 激活、进入某个环境 source activate myenv  
- 退出环境 source deactivate  
- 删除环境 conda remove --name myenv --all  
- 查看当前的环境列表 conda info --envs or $ conda env list  
- 查看某个环境下安装的库 conda list -n myenv  
- 查找包 conda search XXX  
- 安装包 conda install XXX  
- 更新包 conda update XXX  
- 删除包 conda remove XXX  
- 安装到指定环境 conda install -n myenv XXX  
- 生成依赖文件  conda env export --name <环境名称> --file environment.yml  



