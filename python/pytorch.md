# pytorch
## in-place
任何在原地(in-place)改变张量的操作都有一个’_’后缀。将改变自身值！
y.add_(x)   <=>   y = y + x
## numpy数组与torch张量的相互转化
以下的转化中，Torch张量和numpy数组将共享潜在的内存，改变其中一个也将改变另一个。
```python
# numpy to torch
b = torch.from_numpy(a)
# torch to numpy
b = a.numpy()
```
## CUDA张量
使用.cuda函数可以将张量移动到GPU上。
```python
# let us run this cell only if CUDA is available
if torch.cuda.is_available():
    x = x.cuda()
    y = y.cuda()
    x + y
# use ``torch.device`` objects to move tensors in and out of GPU
if torch.cuda.is_available():
    device = torch.device("cuda")          # a CUDA device object
    y = torch.ones_like(x, device=device)  # directly create a tensor on GPU
    x = x.to(device)                       # or just use strings ``.to("cuda")``
    z = x + y
    print(z)
    print(z.to("cpu", torch.double))       # ``.to`` can also change dtype together!
```
## Autograd
### Tensor
```python
tensor(data, dtype=None, device=None, requires_grad=False)
 
参数:
    data： (array_like): tensor的初始值. 可以是列表，元组，numpy数组，标量等;
    dtype： tensor元素的数据类型
    device： 指定CPU或者是GPU设备，默认是None
    requires_grad：是否可以求导，即求梯度，默认是False，即不可导的
```
torch.Tensor是autograd包的核心类。
tensor的属性`.requires_grad`为`True`时会对该张量上的运算进行跟踪，即可导。当你调用`.backward()`时就可以自动求导了，而且导数会被累积到`.grad`属性中。

要阻止张量跟踪历史记录，可以调用`.detach()`将其从计算历史记录中分离出来，并防止将来的计算被跟踪。也可以使用`torch.no_grad()`包装代码块：在评估模型时，这可能特别有用，因为模型可能具有`requires_grad = True`的可训练参数，但我们不需要梯度。

另一个比较核心的类是Function类
Tensor和Function是相互联系的,并形成一个非循环图来构建一个完整的计算过程。每个变量有一个`.grad_fn`属性,它指向创建该变量的一个Function，除了用户自己创建的变量，其`grad_fn`属性为None。

如果你想计算导数,可以在被求导tensor上调用`.backward()`。如果一个tensor是一个标量(只有一个元素值)，便不必给`.backward()`方法指定任何的参数，但是若该tensor有多个元素值，你需要指定一个和该tensor相同形状的grad_output参数。
```python
# 定义requires_grad=True的tensor并附加一个运算
x = torch.ones(2, 2, requires_grad=True)
# 定义的y是运算的结果，故有grad_fn属性
y = x + 2
print(y.grad_fn)
print(x.grad_fn)
# 这里x是用户创建的，所以没有grad_fn属性，上述输出为：
# <AddBackward0 object at 0x000002862AC9EC50>
# None
# 进行更多操作：
z = y * y * 3
out = z.mean()
out.backward()
print(x.grad)
print(y.grad)
# 要计算x梯度，需要对被求导函数进行.backward()
# 当被求导函数为标量时，backward方法不需要任何参数
# 但是中间变量y的导数默认是不计算的
# tensor([[4.5000, 4.5000],
#         [4.5000, 4.5000]])
# None
```
上面的代码只是针对被求导函数为标量时，为向量时，还需要对backward方法的参数进行修改。而且上述代码中因为没有设置backward的参数，只能求一次导（需要改retain_graph=True参数，而且改了以后还是重复累加的求导值）,得到的结果是J^T^*v

```python
# 向量对向量求导
x = torch.tensor([[2.0,3.0]], requires_grad=True)
y = torch.zeros(1, 2)
y[0, 0] = x[0, 0] ** 2 + x[0, 1]
y[0, 1] = x[0, 1] ** 2 + x[0, 0]

# 这里backward后面的向量参数v相当于对结果矩阵的乘积J^T*v，表现类似于排列组合
# 结果矩阵应该是：[[4, 1];[1, 6]]
# y.backward(torch.FloatTensor([[1.0, 1.0]]))
y.backward(torch.FloatTensor([[1.0, 0], [0, 1.0]]))
print(x.grad)

# 矩阵对矩阵求导 aa = w*a;求导后得w
a = torch.tensor([[2.0,3.0],[4.0, 5.0]], requires_grad=True)
w = torch.tensor([[2.0,1.0],[1.0, 2.0]])
aa = torch.mm(w, a)

aa.backward(torch.FloatTensor([[1.0, 0], [0, 1.0]]))
print(a.grad)
```
### backward方法参数：
#### gradient参数
```python
backward(gradient=None, retain_graph=None, create_graph=False)
```
以上例子相当于加了一个gradient参数。gradient参数的维度与最终的函数y保持一样的形状，每一个元素表示当前这个元素所对应的权值。

#### retain_graph参数
一个计算图在进行反向求导之后，为了节省内存，这个计算图就销毁了。 如果你想再次求导，就会报错。
通过设置 retain_graph=True来保留计算图，但是会吃内存，在绝大部分情况下不使用。
#### create_graph高阶导数
用于计算高阶导数
## 神经网络模块
### 定义前向网络
首先是两个通用模块介绍：

```python
class torch.nn.Conv2d(in_channels, out_channels, kernel_size, stride=1, padding=0, dilation=1, groups=1, bias=True)
```
padding(int or tuple, optional) - 输入的每一条边补充0的层数
dilation(int or tuple, optional) – 卷积核元素之间的间距（ "à trous"算法）空洞卷积
groups(int, optional) – 卷积核分组

```python
class torch.nn.MaxPool2d(kernel_size, stride=None, padding=0, dilation=1, return_indices=False, ceil_mode=False)
```
return_indices - 如果等于True，会返回输出最大值的序号，对于上采样操作会有帮助
ceil_mode - 如果等于True，计算输出信号大小的时候，得到的feature map尺寸会使用向上取整（5\*5特征操作后变3\*3），代替默认的向下取整（5\*5变2\*2）的操作

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

# 以LeNet为例：
class Net(nn.Module):

    def __init__(self):
        super(Net, self).__init__()
        # 1 输入图像通道, 第一层输出6通道, 使用3x3 kernel 
        self.conv1 = nn.Conv2d(1, 6, 3)
        self.conv2 = nn.Conv2d(6, 16, 3)
        # an affine operation: y = Wx + b
        self.fc1 = nn.Linear(16 * 6 * 6, 120)  # 6*6为图像尺寸
        self.fc2 = nn.Linear(120, 84)
        self.fc3 = nn.Linear(84, 10)

    def forward(self, x):
        # (2, 2) window max pooling
        x = F.max_pool2d(F.relu(self.conv1(x)), (2, 2))
        # 如果长宽相同，也可以只指定一个kernel参数
        x = F.max_pool2d(F.relu(self.conv2(x)), 2)
        # 将feature map拉成一维向量
        x = x.view(-1, self.num_flat_features(x))
        x = F.relu(self.fc1(x))
        x = F.relu(self.fc2(x))
        x = self.fc3(x)
        return x
    # 计算一张图的某一层feature map总长度
    def num_flat_features(self, x):
        # 只取除去第一维度（batch size）之后的其他维度尺寸
        size = x.size()[1:]
        num_features = 1
        for s in size:
            num_features *= s
        return num_features


net = Net()
print(net)
```
仅仅需要定义一个forward函数就可以了，backward会自动地生成。

模型中可学习的参数会由net.parameters()返回。
```python
params = list(net.parameters())
print(len(params))
print(params[0].size())  # conv1's weight
print(params[1].size())  # conv1's bias
# 输出为 10； torch.Size([6, 1, 3, 3])
# 一共2卷积层，3全连层，每层一个filter一个bias参数
```
`torch.nn`接受的输入为 Samples x nChannels x Height x Width。
单个图片输入需要使用`input.unsqueeze(0)`来加一个batch维度。

```python
input = torch.randn(1, 1, 32, 32)    # 随机输入一输出
out = net(input)
# 按道理应该是下面这个表达式调用前向传播，但应该是在__call__函数中有调用
# out=net.forward(input)  # 这个表达式也可以用
net.zero_grad()    # 对所有的参数的梯度缓冲区进行归零
out.backward(torch.randn(1, 10))    # 使用随机的梯度进行反向传播
```
### 计算Loss function
代价函数接受（输出，目标）对的输入，并计算估计出输出与目标之间的差距。在nn模块下有许多损失函数可利用。

```python
output = net(input)
target = torch.randn(10)  # a dummy target, for example
target = target.view(1, -1)  # make it the same shape as output
criterion = nn.MSELoss()

loss = criterion(output, target)
print(loss)
```
### 反向传播
现在，如果你跟随loss从后往前看，使用.grad_fn属性可以看到这样的一个计算流程图：
```
input -> conv2d -> relu -> maxpool2d -> conv2d -> relu -> maxpool2d  
      -> view -> linear -> relu -> linear -> relu -> linear 
      -> MSELoss
      -> loss
```
因此当我们调用loss.backward()时整个图通过代价来进行区分，图中所有的变量都会以.grad来累积梯度。

```python
net.zero_grad()     # zeroes the gradient buffers of all parameters

print('conv1.bias.grad before backward')
print(net.conv1.bias.grad)

loss.backward()

print('conv1.bias.grad after backward')
print(net.conv1.bias.grad)
```
### 更新权重
结合更新权重，完整如下：

```python
import torch.optim as optim
# create your optimizer
optimizer = optim.SGD(net.parameters(), lr = 0.01)

# in your training loop:
optimizer.zero_grad()               # zero the gradient buffers
output = net(input)                 # 正向传播
loss = criterion(output, target)    # 计算loss
loss.backward()                     # 反向传播
optimizer.step() # Does the update  # 更新权重
```
## 用torchvision的数据处理
先用Pillow，OpenCV等库将图片转化为numpy数组，载转为torch.tensor。
另外还有torchvision这个包可用，其中包含了一些现成的数据集如：Imagenet, MNIST, CIFAR10等等。同时还有一些转换图像用的工具。 这非常的方便并且避免了写样板代码。

这类数据可以分批调用的原因是，在类中有一个`  def __getitem__(self, index):`方法。自己制作数据集时要定义这个方法。

```python
# 以cifar10为例
transform = transforms.Compose(
    [transforms.ToTensor(),
     transforms.Normalize((0.5, 0.5, 0.5), (0.5, 0.5, 0.5))])

trainset = torchvision.datasets.CIFAR10(root='./data', train=True,
                                        download=True, transform=transform)
trainloader = torch.utils.data.DataLoader(trainset, batch_size=4,
                                          shuffle=True, num_workers=2)

testset = torchvision.datasets.CIFAR10(root='./data', train=False,
                                       download=True, transform=transform)
testloader = torch.utils.data.DataLoader(testset, batch_size=4,
                                         shuffle=False, num_workers=2)

classes = ('plane', 'car', 'bird', 'cat',
           'deer', 'dog', 'frog', 'horse', 'ship', 'truck')
```
### 训练
```python
import torch.optim as optim

criterion = nn.CrossEntropyLoss()
optimizer = optim.SGD(net.parameters(), lr=0.001, momentum=0.9)

for epoch in range(2):  # loop over the dataset multiple times

    running_loss = 0.0
    for i, data in enumerate(trainloader, 0):
        # get the inputs; data is a list of [inputs, labels]
        inputs, labels = data

        # zero the parameter gradients
        optimizer.zero_grad()

        # forward + backward + optimize
        outputs = net(inputs)
        loss = criterion(outputs, labels)
        loss.backward()
        optimizer.step()

        # print statistics
        running_loss += loss.item()
        if i % 2000 == 1999:    # print every 2000 mini-batches
            print('[%d, %5d] loss: %.3f' %
                  (epoch + 1, i + 1, running_loss / 2000))
            running_loss = 0.0

print('Finished Training')
```
### Test
```python
correct = 0
total = 0
# 测试时要保持梯度不再传递
with torch.no_grad():
    for data in testloader:
        images, labels = data
        outputs = net(images)
        _, predicted = torch.max(outputs.data, 1)
        total += labels.size(0)
        correct += (predicted == labels).sum().item()

print('Accuracy of the network on the 10000 test images: %d %%' % (
    100 * correct / total))
```
### 类别性能分析
```python
class_correct = list(0. for i in range(10))
class_total = list(0. for i in range(10))
with torch.no_grad():
    for data in testloader:
        images, labels = data
        outputs = net(images)
        _, predicted = torch.max(outputs, 1)
        c = (predicted == labels).squeeze()
        for i in range(4):
            label = labels[i]
            class_correct[label] += c[i].item()
            class_total[label] += 1


for i in range(10):
    print('Accuracy of %5s : %2d %%' % (
        classes[i], 100 * class_correct[i] / class_total[i]))
```
### GPU上训练
```python
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
# 将模型传到GPU上训练
net.to(device)
# 记住输入和目标也需要传到GPU上，需要赋值给新的张量
# 也就是说for循环中，不断喂数据的之前，需要把数据先传到GPU上
inputs, labels = data[0].to(device), data[1].to(device)
```
### 多GPU运行
主要是依靠`model = nn.DataParallel(model)`

```python
model = Model(input_size, output_size)
if torch.cuda.device_count() > 1:
  print("Let's use", torch.cuda.device_count(), "GPUs!")
  # dim = 0 [30, xxx] -> [10, ...], [10, ...], [10, ...] on 3 GPUs
  model = nn.DataParallel(model)

model.to(device)

# Run the model 这一步正常如前
for data in dataset:
    input = data.to(device)
    output = model(input)
```
## CSV读取
可用csv模块或者pandas模块

```python
landmarks_frame = pd.read_csv('data/faces/face_landmarks.csv')

# 取第一个样本，第一列为图片名字，后面为平铺的二维数组
# iloc基于索引位来选取数据集，0:4就是选取 0，1，2，3这四行
# 需要注意的是这里是前闭后开集合
n = 0
img_name = landmarks_frame.iloc[n, 0]
landmarks = landmarks_frame.iloc[n, 1:].as_matrix()
landmarks = landmarks.astype('float').reshape(-1, 2)
```
## 从一堆图片自己制作数据集
`torch.utils.data.Dataset`是专门用于抽取数据的类。自己制作的数据集需要继承自该类。
制作的数据集一般包括以下几个成员：
`__len__` 返回数据集长度
`__getitem__` 以支持按索引取数据集

```python
from torch.utils.data import Dataset

class FaceLandmarksDataset(Dataset):
    """人脸定位点数据集"""

    def __init__(self, csv_file, root_dir, transform=None):
        """
        Args:
            csv_file (string): 标注目录
            root_dir (string): 图片目录
            transform (callable, optional): 可选的对图片进行的变化
        """
        self.landmarks_frame = pd.read_csv(csv_file)
        self.root_dir = root_dir
        self.transform = transform

    def __len__(self):
        return len(self.landmarks_frame)

    def __getitem__(self, idx):
        img_name = os.path.join(self.root_dir,
                                self.landmarks_frame.iloc[idx, 0])
        image = io.imread(img_name)
        landmarks = self.landmarks_frame.iloc[idx, 1:].as_matrix()
        landmarks = landmarks.astype('float').reshape(-1, 2)
        sample = {'image': image, 'landmarks': landmarks}

        if self.transform:
            sample = self.transform(sample)

        return sample
```
定义一些对图片的操作，这里定义成类的形式，而不是函数方便调用：

```python
class Rescale(object):
    """Rescale the image in a sample to a given size.

    Args:
        output_size (tuple or int): Desired output size.
        If tuple, output is matched to output_size. 
        If int, smaller of image edges is matched  to output_size keeping aspect ratio the same.
    """

    def __init__(self, output_size):
        assert isinstance(output_size, (int, tuple))
        self.output_size = output_size

    def __call__(self, sample):
        image, landmarks = sample['image'], sample['landmarks']

        h, w = image.shape[:2]
        if isinstance(self.output_size, int):
            if h > w:
                new_h, new_w = self.output_size * h / w, self.output_size
            else:
                new_h, new_w = self.output_size, self.output_size * w / h
        else:
            new_h, new_w = self.output_size

        new_h, new_w = int(new_h), int(new_w)

        img = transform.resize(image, (new_h, new_w))

        # h and w are swapped for landmarks because for images,
        # x and y axes are axis 1 and 0 respectively
        landmarks = landmarks * [new_w / w, new_h / h]

        return {'image': img, 'landmarks': landmarks}


class RandomCrop(object):
    """Crop randomly the image in a sample.

    Args:
        output_size (tuple or int): Desired output size. If int, square crop
            is made.
    """

    def __init__(self, output_size):
        assert isinstance(output_size, (int, tuple))
        if isinstance(output_size, int):
            self.output_size = (output_size, output_size)
        else:
            assert len(output_size) == 2
            self.output_size = output_size

    def __call__(self, sample):
        image, landmarks = sample['image'], sample['landmarks']

        h, w = image.shape[:2]
        new_h, new_w = self.output_size

        top = np.random.randint(0, h - new_h)
        left = np.random.randint(0, w - new_w)

        image = image[top: top + new_h,
                      left: left + new_w]

        landmarks = landmarks - [left, top]

        return {'image': image, 'landmarks': landmarks}


class ToTensor(object):
    """Convert ndarrays in sample to Tensors."""

    def __call__(self, sample):
        image, landmarks = sample['image'], sample['landmarks']

        # swap color axis because
        # numpy image: H x W x C
        # torch image: C X H X W
        image = image.transpose((2, 0, 1))
        return {'image': torch.from_numpy(image),
                'landmarks': torch.from_numpy(landmarks)}
```
实例化调用：(可以看出确实比函数更方便，这样数据集和参数不必每次都调用)

```python
from torchvision import transforms

scale = Rescale(256)
crop = RandomCrop(128)
# Composed是torchvision中可以用来将对图片的操作复合起来的操作
composed = transforms.Compose([Rescale(256),
                               RandomCrop(224)])

# Apply each of the above transforms on sample.
fig = plt.figure()
sample = face_dataset[65]
for i, tsfrm in enumerate([scale, crop, composed]):
    transformed_sample = tsfrm(sample)

    ax = plt.subplot(1, 3, i + 1)
    plt.tight_layout()
    ax.set_title(type(tsfrm).__name__)
    show_landmarks(**transformed_sample)

plt.show()
```
在整个数据集上迭代：

```python
from torch.utils.data import DataLoader
from torchvision import utils

transformed_dataset = FaceLandmarksDataset(
                        csv_file='data/facesface_landmarks.csv',
                        root_dir='data/faces/',
                        transform=transforms.Compose([
                                               Rescale(256),
                                               RandomCrop(224),
                                               ToTensor()
                                           ]))

# 利用dataloader分批，打乱数据
dataloader = DataLoader(transformed_dataset, batch_size=4,
                        shuffle=True, num_workers=4)

# Helper function to show a batch
def show_landmarks_batch(sample_batched):
    """Show image with landmarks for a batch of samples."""
    images_batch, landmarks_batch = \
            sample_batched['image'], sample_batched['landmarks']
    batch_size = len(images_batch)
    im_size = images_batch.size(2)
    grid_border_size = 2

    grid = utils.make_grid(images_batch)
    plt.imshow(grid.numpy().transpose((1, 2, 0)))

    for i in range(batch_size):
        plt.scatter(landmarks_batch[i, :, 0].numpy() + i * im_size + (i + 1) * grid_border_size,
                    landmarks_batch[i, :, 1].numpy() + grid_border_size,
                    s=10, marker='.', c='r')

        plt.title('Batch from dataloader')

for i_batch, sample_batched in enumerate(dataloader):
    print(i_batch, sample_batched['image'].size(),
          sample_batched['landmarks'].size())

    # observe 4th batch and stop.
    if i_batch == 3:
        plt.figure()
        show_landmarks_batch(sample_batched)
        plt.axis('off')
        plt.ioff()
        plt.show()
        break
```
## 调整学习率lr_scheduler 
torch.optim.lr_scheduler 中提供了基于多种epoch数目调整学习率的方法，主要介绍以下积几类。
### lr_scheduler.StepLR
```python
import torch
import torch.nn.functional as F

optimizer = optim.Adam(classifier.parameters(), lr=0.001, betas=(0.9, 0.999))
scheduler = lr_scheduler.StepLR(optimizer, step_size=30, gamma=0.1)
for epoch in range(opt.nepoch):
    scheduler.step()    # 更新学习率
    for i, data in enumerate(dataloader, 0):
        points, target = data
        optimizer.zero_grad()
        pred = net(points)
        loss = F.nll_loss(pred, target)
        loss.backward()
        optimizer.step()    # 更新模型
        if i % 10 == 0:
            print('[%d: %d/%d] %s loss: %f accuracy: %f' % (epoch, i, num_batch, blue('test'), loss.item(), correct.item()/float(opt.batchSize)))
```
`scheduler = lr_scheduler.StepLR(optimizer, step_size=30, gamma=0.1)`表示每30个epoch，学习率乘以0.1。
### lr_scheduler.MultiStepLR
`scheduler = lr_scheduler.MultiStepLR(optimizer, [30, 80], 0.1)`表示以30, 80为节点，学习率乘以0.1。
### lr_scheduler.ExponentialLR
`scheduler = lr_scheduler.ExponentialLR(optimizer, gamma=0.9)`指数下降。
### lr_scheduler.LambdaLR
```python
lambda1 = lambda epoch: epoch // 30     # 每过30个epoch加0.05
lambda2 = lambda epoch: 0.95 ** epoch      # lr = 0.95^epoch
scheduler = LambdaLR(optimizer, lr_lambda=[lambda1, lambda2]) 
```
lambda表示的是一个计算学习率的函数；或者一个函数list，有几个学习率就写几个lambda
## 矩阵乘法
### torch.mm
最不常用,不能广播,只能用于(n \* m) 与 (m \* p)的矩阵乘法
### torch.bmm
batch间的矩阵乘法，不改变第一个即batch维度下的矩阵乘法
(b × n × m) 与 (b × m × p) 得 (b × n × p)
![B*N*3](_v_images/20190802195314670_197434580.png)
### torch.matmul
最终结果取决于输入维度:
1. 输入均一维: 得标量
2. 输入均二维: 矩阵乘
3. 如果两个输入均至少一维,并至少有一个输入维度大于2,那么得到的是batch间的乘法.非参与矩阵乘的维度如果是 1 ,是用于凑数满足矩阵乘法的,在运算后可以去掉 e.g: (j × 1 × n × m) 与 (k × m × p) 得 (j × k × n × p)
## 已知索引求原始点数据
```python
def index_points(points, idx):
    """
    输入:
        points: 输入点云数据, [B, N, C]
        idx: 采样点索引, [B, S]
    输出:
        new_points:, 采样点原始数据, [B, S, C]
    """
    device = points.device
    B = points.shape[0]
    view_shape = list(idx.shape)
    # 将除batch维度的其他维度改为1
    view_shape[1:] = [1] * (len(view_shape) - 1)
    repeat_shape = list(idx.shape)
    # 仅将batch维度改为1
    repeat_shape[0] = 1
    # 其实就是想把[0, B-1]的range一维向量变为与idx维度相同的[0, B-1]重复S次矩阵,这样维度相同切片时才能一一对应
    batch_indices = torch.arange(B, dtype=torch.long).to(device).view(view_shape).repeat(repeat_shape)
    new_points = points[batch_indices, idx, :]
    return new_points
```
## torch.gather
```python
"""
    就是dim是几,就在该位置用输入的index顺序填充,不同的括号表示dim+1
    For a 3-D tensor the output is specified by:

    out[i][j][k] = input[index[i][j][k]][j][k]  # dim=0
    out[i][j][k] = input[i][index[i][j][k]][k]  # dim=1
    out[i][j][k] = input[i][j][index[i][j][k]]  # dim=2
"""
b = torch.Tensor([[1,2,3],[4,5,6]])
print(b)
index_1 = torch.LongTensor([[0,1],[2,0]])
index_2 = torch.LongTensor([[0,1,1],[0,0,0]])
print(torch.gather(b, dim=1, index=index_1))
print(torch.gather(b, dim=0, index=index_2))
```
index实际上是索引，具体是行还是列的索引要看前面dim 的指定;
index1时指定dim=1，也就是横向，那么索引就是列号。index的大小就是输出的大小!
输出:
```python
 1  2  3
 4  5  6
[torch.FloatTensor of size 2x3]

 1  2
 6  4
[torch.FloatTensor of size 2x2]

 1  5  6
 1  2  3
[torch.FloatTensor of size 2x3]
```
## 从N个点中随机选取K个点
```python
# 会重复选取
pre_ind = torch.randint(low=0, high=N, size=(B, self.P), dtype=torch.long).to(pts.device)
represent_pts = index_points(pts, pre_ind)
# 不重复选取,但无法得到选取点ind
idx = np.random.choice(pts.size()[1], self.P, replace=False).tolist() # .to(pts.device)
represent_pts = pts[:, idx, :]
# 不重复选取
sqrdists = square_distance(pts, xyz)    # (B,P,N)
# 选取最小的前K*D个
# k_ind = torch.topk(sqrdists, k=K*D, dim=-1, largest=False)[1]
k_ind = sqrdists.sort(dim=-1)[1][:, :, :K*D]  # (B,P,K*D)
rand_columns = torch.randperm(K*D, dtype=torch.long)[:K].to(device)
k_ind = k_ind[:, :, rand_columns]
# 还是会重复
B, N, C, M = 2, 3, 4, 5
x = torch.randn(B, N, C)
idx = torch.randint(0, C, (B, N, M))
torch.gather(x, 2, idx)
```