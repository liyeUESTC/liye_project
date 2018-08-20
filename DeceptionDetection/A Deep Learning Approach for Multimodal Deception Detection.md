### A Deep Learning Approach for Multimodal Deception Detection

视频分析识别说谎


二分类问题（Deceptive AND Truthful）

(1)输入多模态信息，语音、面部表情、动作、文字
(2)使用同一个网络处理多模态信息

准确率达到96.14




动作
声音
脸部表情

模型

多模态信息输入，分别处理，得到对应特征向量，做拼接后，输出二分类。


(1)视频
得到300维特征向量
3D卷积

input->conv(32* 3 * 5 * 5 * 5)->pooling->fc->softmax
特征向量vf的维度是300

(2)文本
得到300维特征向量
Text-CNN

(3)音频：使用openSMILE提取音频特征
先使用音频工具SoX (Sound eXchange)去除背景噪声，并且做Z-standardization标准化。
送入openSMILE提取音频特征，得到6373维的音频特征。
最后接一个多层感知机得到300维的特征向量af。

(4)微表情
得到39维的特征向量


多模态融合

(1)Concatenation
直接拼接，得到939维特征向量
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180820150331.png)
(2)Handamard + Concatenation
- 前三个特征向量做Handamard运算（对应位置元素相乘），得到一个300维向量
- 300维向量与39维向量拼接，得到339维向量
![](https://github.com/liyeUESTC/liye_project/blob/file_paper/images/QQ%E6%88%AA%E5%9B%BE20180820160644.png)






latter fusion
