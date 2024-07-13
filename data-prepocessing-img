"""
resample
输入
img_obj
"""
def resample(img_obj):###把路径输进去
    spacing = np.round(img_obj.GetSpacing(),decimals=4)
    origin = img_obj.GetOrigin()
    direction = img_obj.GetDirection()
    input_size = img_obj.GetSize()##输入的size应该都是一样的
    output_spacing = [1, 1, 1]
    output_size = [int(np.round((input_size[i] * spacing[i])) / output_spacing[i]) for i in range(3)]
    transform = sitk.Transform(3, sitk.sitkIdentity) #what's mean?
    ##如果是ROI  AP sitk.sitkNearestNeighbor
    interp = sitk.sitkLinear  #是Im是线性插值，是label是最近邻插值
    resampled_img = sitk.Resample(img_obj, output_size, transform,
        interp, origin, output_spacing, direction)
    return resampled_img
"""
dcm转nii，加快Load速度
输入是
dcms_path:dcm路径
nii_path:.nii保存路径
"""
def dcm2nii(dcms_path, nii_path):
    # 1.构建dicom序列文件阅读器，并执行（即将dicom序列文件“打包整合”）
    reader = sitk.ImageSeriesReader()
    dicom_names = reader.GetGDCMSeriesFileNames(dcms_path)
    reader.SetFileNames(dicom_names)
    image2 = reader.Execute()
    # 2.将整合后的数据转为array，并获取dicom文件基本信息
    image_array = sitk.GetArrayFromImage(image2)    # z, y, x
    origin = image2.GetOrigin()     # x, y, z
    spacing = image2.GetSpacing()   # x, y, z
    direction = image2.GetDirection()       # x, y, z
    # 3.将array转为img，并保存为.nii.gz
    image3 = sitk.GetImageFromArray(image_array)
    image3.SetSpacing(spacing)
    image3.SetDirection(direction)
    image3.SetOrigin(origin)
    sitk.WriteImage(image3, nii_path)
"""
截取中间区域
输入
img_array: array
输出：
img_center: array
"""
def center(img_array):
    D, H, W = img_array.shape 
    d_idx, h_idx, w_idx = np.where(img_array > 0)###将大于0的区域保存下来
    d0, d1 = d_idx.min(), d_idx.max() 
    h0, h1 = h_idx.min(), h_idx.max() 
    w0, w1 = w_idx.min(), w_idx.max() 
    # print("ROI区域",d0,d1,h0,h1,w0,w1)
    img_center = img_array[d0:d1, h0:h1, w0:w1]
    return img_center
