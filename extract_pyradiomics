def catch_features(imagePath,maskPath):

    if imagePath is None or maskPath is None:  # Something went wrong, in this case PyRadiomics will also log an error
        raise Exception('Error getting testcase!')  # Raise exception to prevent cells below from running in case of "run all"
    settings = {}
    settings['binWidth'] = 25  # 5
    settings['sigma'] = [3, 5]
    settings['Interpolator'] = sitk.sitkBSpline
    settings['resampledPixelSpacing'] = [1, 1, 1]  # 3,3,3
    settings['voxelArrayShift'] = 1000  # 300
    settings['normalize'] = True
    settings['normalizeScale'] = 100
    extractor = featureextractor.RadiomicsFeatureExtractor(**settings)
    #extractor = featureextractor.RadiomicsFeatureExtractor()
    print('Extraction parameters:\n\t', extractor.settings)

    extractor.enableImageTypeByName('LoG')
    extractor.enableImageTypeByName('Wavelet')
    extractor.enableAllFeatures()
    extractor.enableFeaturesByName(firstorder=['Energy', 'TotalEnergy', 'Entropy', 'Minimum', '10Percentile', '90Percentile', 'Maximum', 'Mean', 'Median', 'InterquartileRange', 'Range', 'MeanAbsoluteDeviation', 'RobustMeanAbsoluteDeviation', 'RootMeanSquared', 'StandardDeviation', 'Skewness', 'Kurtosis', 'Variance', 'Uniformity'])
    extractor.enableFeaturesByName(shape=['VoxelVolume', 'MeshVolume', 'SurfaceArea', 'SurfaceVolumeRatio', 'Compactness1', 'Compactness2', 'Sphericity', 'SphericalDisproportion','Maximum3DDiameter','Maximum2DDiameterSlice','Maximum2DDiameterColumn','Maximum2DDiameterRow', 'MajorAxisLength', 'MinorAxisLength', 'LeastAxisLength', 'Elongation', 'Flatness'])
# 上边两句我将一阶特征和形状特征中的默认禁用的特征都手动启用，为了之后特征筛选
    print('Enabled filters:\n\t', extractor.enabledImagetypes)
    feature_cur = []
    feature_name = []
    result = extractor.execute(imagePath, maskPath)
    for key, value in six.iteritems(result):
        print('\t', key, ':', value)
        feature_name.append(key)
        feature_cur.append(value)
    print(len(feature_cur[37:]))
    name = feature_name[37:]
    name = np.array(name)
    '''
    flag=1
    if flag:
        name = np.array(feature_name)
        name_df = pd.DataFrame(name)
        writer = pd.ExcelWriter('key.xlsx')
        name_df.to_excel(writer)
        writer.save()
        flag = 0
    '''
    for i in range(len(feature_cur[37:])):
        #if type(feature_cur[i+22]) != type(feature_cur[30]):
        feature_cur[i+37] = float(feature_cur[i+37])
    return feature_cur[37:],name
def read_img(img_name,seg_name):
    i = 0
    id = []
    save_file = np.empty(shape=[1,1052])

    ##排序，
    path_list = os.listdir(seg_name)
    path_list.sort(key=lambda x:int(x[:-6]))
    for filename in path_list: 
        seg_path = seg_name+'/'+filename
        print(seg_path)
        ###得到betti_curve的矩阵
        # gray = get_img_as_gray_array(filename1)
    
        ##得到3D图像
        # gray = GetRoi_topo(filename1)
        ##得到2D图像
        filename1 = img_name +'/' +filename[:-4] +'.nii.gz'
        print(filename1)
        try:
            image_obj = sitk.ReadImage(filename1)
            image = sitk.GetArrayFromImage(image_obj)
            seg_obj = sitk.ReadImage(seg_path)
            seg = sitk.GetArrayFromImage(seg_obj)

            ##filename1为图片路径，seg_path为roi路径
            save_curdata,name = catch_features(filename1,seg_path)
            name = np.insert(name,0,'label')
            save_curdata = np.array(save_curdata)

            save_curdata = np.insert(np.array(save_curdata),0,(int)(filename.split('.')[0].split('-')[1]))
            save_curdata = save_curdata.reshape([1, 1052])
            id.append(filename.split('-')[0])

            save_file = np.append(save_file,save_curdata,axis=0)
            print(save_file.shape)

        except:
            print("something wrong")


    save_file = np.delete(save_file,0,0)
    #save_file = save_file.transpose()
    #print(save_file.shape)
    id_num = len(id)
    id = np.array(id)
    name_df = pd.DataFrame(save_file)
    name_df.index = id
    name_df.columns = name
    writer = pd.ExcelWriter('NSCLC-Radiomics-features-new.xlsx')
    name_df.to_excel(writer)
    writer.save()
