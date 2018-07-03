**  openmpi是用来加速caffe训练的 **
**  初次使用openmpi网址：https://github.com/yjxiong/caffe **

## 卸载openmpi

查看openmpi版本：runmpi --version  （--with-cuda） # 编译openmpi的cuda接口，才可以调用使用openmpi调用caffe

- 删掉安装目录

rm -rf /usr/local/openmpi

- 用apt-get删掉openmpi

sudo apt-get remove openmpi-common



## 安装openmpi

```
- 下载
wget https://www.open-mpi.org/software/ompi/v2.0/downloads/openmpi-2.0.2.tar.gz
tar zxvf openmpi-2.0.2.tar.gz
cd openmpi-2.0.2
sudo ./configure --prefix=/usr/local

- 编译安装
sudo make
sudo make install 

- 配置环境变量（~/.bashrc）
export PATH=$PATH:/usr/local/openmpi/bin  
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/openmpi/lib/  
source ~/.bashrc  
sudo ldconfig  

- 测试
cd examples
make
mpirun -np 8 hello_c
不报错说明安装成功
```

## 运行mpi

mpirun -np 4 ./install/bin/caffe train --solver=<Your Solver File> [--weights=<Pretrained caffemodel>]

实测：mpirun -np 2 ./build/tools/caffe train --solver=examples/mnist/lenet_solver.prototxt
