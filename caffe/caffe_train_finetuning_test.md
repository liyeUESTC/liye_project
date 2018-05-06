### Train

- Solver_r2.prototxt：配置参数

- train_resnet18_r2.prototxt：模型结构

- c3d_resnet18_sports1m_r2_iter_2800000.caffemodel：训练好的网络权值

makdir -p LOG_TRAIN   # 创建日志文件夹

GLOG_log_dir="./LOG_TRAIN"   #caffe提供了保存训练日志以及解析训练和测试数据的功能，该步骤为保存训练日志

../../build/tools/caffe.bin train 

--solver=solver_r2.prototxt 



### Finetuning

- Solver_r2.prototxt：配置参数

- train_resnet18_r2.prototxt：模型结构

- c3d_resnet18_sports1m_r2_iter_2800000.caffemodel：训练好的网络权值

GLOG_log_dir="./LOG_TRAIN" 

../../build/tools/caffe.bin train 

--solver=solver_r2.prototxt 

--weights=c3d_resnet18_sports1m_r2_iter_2800000.caffemodel 

--gpu=0



### Test



- train_resnet18_r2.prototxt：模型结构

- c3d_resnet18_ucf101_r2_ft_iter_20000.caffemodel：训练好的网络权值

- stage='test-on-val'：在验证集上进行测试，网络结构中会定义在验证集上测试的layer和在训练集上测试的layer。

GLOG_logtostderr=1 

../../build/tools/caffe.bin test 

--model=train_resnet18_r2.prototxt 

--weights=c3d_resnet18_ucf101_r2_ft_iter_20000.caffemodel 

--stage='test-on-val' --iterations=2092 --gpu=0


