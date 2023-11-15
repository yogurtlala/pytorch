import medmnist
from medmnist import INFO, Evaluator
##dataloader
data_flag = args.public_data ##nodulemnist
info = INFO[data_flag]##print the dataset details given a subset flag
DataClass = getattr(medmnist,info['python_class'])
print(f"MedMNIST v{medmnist.__version__} @ {medmnist.HOMEPAGE}")

##load data
train_dataset = DataClass(split = 'train',transform = train_transforms,root='/data/pst/code/medmnist')##路径
train_loader = data.DataLoader(dataset = train_dataset,batch_size=args.bs)
test_dataset = DataClass(split = 'val',transform = test_transforms,root='/data/pst/code/medmnist')
test_loader = data.DataLoader(dataset = test_dataset,batch_size=args.bs)
