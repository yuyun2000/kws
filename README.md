KWS
-

- 数据增强内容有4个：
    1. 将两秒的含有关键字的音频截取一定的长度在和随机的背景音拼接，用来减弱关键词说一半误识别的错误
    2. 添加随机噪音，添加两遍，将数据翻三倍
    3. 在频谱图上随机mask，这里取横向纵向各随机mask两条线
    4. dataset使用mixup，使用tensorflow中dataset.zip的方法创建

使用步骤（以xiaoai文件夹内的小爱同学示例）：
1. 准备数据，在wav文件夹内有男女各15段每段2秒的小爱同学的录音，在bg文件夹内有随机的背景音15段（时长大于2秒）
2. 使用filerename.py修改训练样本的文件名称，以便后续使用
3. 使用dataaug-noise.py,会先把15段背景音的前两秒写入wav文件夹内，再进行噪音扩增，会将45个样本扩增为135个
4. 使用cutmix.py实现第一种增强，只对未加噪声的30个样本增强，此时样本数量由135个扩增为165，关键词90个，背景音75个
5. 使用prepro将wav内的音频预处理为频谱图并保存至pic文件夹内
6. 数据部分处理完毕
---
训练部分
1. writelist.py 自动打算pic内文件顺序并28分开训练测试样本
2. 运行train.py开始训练 测试结果在生成的txt文件中，模型保存在h5文件下
3. 测试通过98

注意：
wav文件夹内放的都是2秒左右的音频，初始录制的长音频需要vad或者手动截取以下；bg文件夹内的背景音只要大于2秒即可。
vad的demo见vad和cut两个文件