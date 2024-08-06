# dicom和dicomdir
转载[http://blog.sina.com.cn/s/blog\_4bce5f4b01019ix5.html](http://blog.sina.com.cn/s/blog_4bce5f4b01019ix5.html "http://blog.sina.com.cn/s/blog_4bce5f4b01019ix5.html")
## DICOM 文件
DICOM 文件内容在 Part 3 DICOM IOD 里定义。CT, MR, CR, DR, US, NM, PET， XA 等各有自己的内容定义，由共同的专有的部分 （IE 和 Modules) 组成。 

DICOMDIR 由 Part 10 定义。是一种 mini 可变长度数据库文件结构。 

过去我手下一个学计算机的非常聪明的印度人问我：图像文件格式不是有 BMP, GIF, TIF 和 JPEG 等等吗，为什么还要定义 DICOM　呢？　我向他解释了好几次最后还是放弃了。 任何图像文件格式无非是由两个部分组成：存参数的 header 和图点数据 （pixel data)。 BMP, JPEG, TIFF 之类的格式的 header 只描述图像的基本参数：如几行、几烈、每点用了几位、有没有压缩、调色板等等。Header 往往是固定长度的。 而医疗影像还要许多其它参数，如病人基本资料、检验基本资料、系列资料、位置资料等等。而且每种 Modality 和每种 image 所需要的内容不一样。因此，一般的图像格式不能用。 

DICOM 的 4 个内容层次： 

1\. Patient (病人) 

2\. Study (检验) 

3\. Series (系列)

4\. Image (图像) 

尽管头几层的内容在很多图像里是相同的，它们在每个图像文件里都要有。 每一层叫一个 Information Entity,或 IE （从 relational database schema 设计引用而来）。每一层又细分成 Module。每个 Module 里面的最小单元叫做一个 attribute 或 element。

现在举个例子：

CR 图像

DICOM Part 3, A.2.3, Table A.2-1 

1\. Patient IE: a. Patient Module (参考 C.7.1.1) 

2\. Study IE: a. Study Module (参考 C.7.2.1) b. Patient Study Module (参考 C.7.2.2) 

3\. Series IE: a. General Series (参考 C.7.3.1) b. CR Series (参考 C.8.1..1) c. General Equipment (参考 C.7.5.1) 

4\. Image IE: a. Genrral Image (C.7.6.1) b. Image Pixel (C.7.6.3) c. Contrast/bolus (C.7.6.4) d. CR Image (C.8.1.2) 

... 

i. SOP Common (C.12.1) 

将这些 modules (tables) 里的所有 elements 都找出来就做成了一个 CR 图像的架构。 要注意的是这些 module 有些是一定要的 (modatory) 有些是用户选用的 (user)。 到了每个 module 里 attribute/element 表又有分五类 Type 1, 1C, 2, 2C 和 3。 Type 1 是一定要的，2 也是一定要的但是内容可以是空的。Type 3 则可要可不要。 所以浓缩一下，一个 CR 图像里的元素 (elements) 也不是太多。

把这些表格展开后，这些 elements 组成一个 dataset。 那么写到一个文件里或通过网路传送又是个什么格式呢？这就要看 Part 5。 

一个元素 (element) 的结构是： 

1\. group tag: 16-bit 

2\. element tag: 16-bit 

3\. length (or VR/length): 32-bit 

4\. data (bytes of length) 

对应每一个用到的 element DICOM 标准 Part 6 都定义了一个 group tag 和 element tag。

比如说： patient name: 0x0010, 0x0010 patient ID: 0x0010, 0x0020 ... 

VR 说的是 element 格式，比如说 patinet name 的 VR 是 PN。格式是 last\_name^first\_name^middle\_name^prefix^surfix。那么我的英文名字就是: Wang^JB^^Dr.^ 

往外写时要做几个事情： 

1\. 要把所有元素按 group tage 和 element tag 理一遍 (sort)。从小排到大。 

2\. 如果是写 DICOM 介质的 DICOM file 还先写 128 bytes preamble (一般是空白),加 "DICM", 加 group 2 Meta header。（讲到 Part 10 时再细说） 

3\. 如果 dataset 里面含有 Sequence elements, sequence 里面每一个 Item 又是一个 dataset。

作业： 有了这些知识，你就可以开始写一个小小的 BMP 到 DICOM 的转化程序。

关键资料： 

Modality (0008, 0060): SC 

Photometric Interpretation (0028, 0004): RGB

SOP Class UID: 1.2.840.10008.1.5.4.1.1.7 

最简单的办法是写一个 structure，然后一个 
~~~ cpp
typedef struct DicomElem(short int group_tag, short int element_tag, char VR[4], int length, char data[128]) DicomElem; 

DicomElem CRDataSet[] = (
    (0x0008, 0x0005, "CS", 10, "ISO_IR 100"), 
    (0x0008, 0x0008, "CS", 16， "ORIGINALPRIMARY"), 
    ...
    (0x0010, 0x0010, "PN", 16, "My^Test^Image^^ ")
); 

void WriteCDImage(FILE* fp)

{
    DicomElem elem = CRDataSet[0];
    unsigned long int lComboTag;
    int nCols, nRows;
    unsigned char* pPixelData unsigned long int lPixelLength;
    pPixelData = LoadBMPImgeData("MyImage.bmp", nCols, nRows, lPixelLength);
    while (CRDataSet.group_tag)
    {
        lComboTag = (CRDataSet.group_tag << 16) | CRDataSet.element_tag;
        switch (lComboTag)
        {
        case 0x00280010:
            *((short int*)CRDataSet.data) = nCols;

            break;

        case 0x00280011:
            *((short int*)CRDataSet.data) = nRows;

            break;

            ... 

        }

        // Write group and element tag 

        fwrite(&lComboTag, 1, sizeof(long), fp);

        // Write VR 

        fwrite(CRDataSet.VR, 1, 2, fp);

        if (lComboTag != 0x7fe00010)

        {

            fwrite(CRDataSet.length, 1, sizeof(short), fp);

            fwrite(CRDataSet.data, 1, CRDataSet.length, fp);

        }

        else

        {

            fwrite("00", 1, 2, fp);

            // Two blank bytes after VR 

            fwrite(&lPixelLength, 1, sizeof(long), fp);

            // Length 

            fwrite(pPixelData, 1, lPixelLength, fp);

        }

        i++;

    }

}

unsigned char* LoadBMPImgeData(char* fileName, int &nCols, int &nRows, unsigned long &lPixelLength) ( .... ) 


~~~
细节自己去写。 

更上一层楼： DICOM 文件读写最难的是两个东西： 

o DICOM Sequence 

o DICOM Pixel Data 

刚刚让大家做的作业里没有 Sequence，pixel data 也很简单。 Sequence 在 C 里的类比是一个 structure 的 array。 是结构套结构。 所以读起来难。更讨厌的是 Sequence 还可以不定义长度， 即长度是 -1。要靠你自己去找 (FFFE, E0DD) 来决定 Sequence 是否结束。 Array 里面的每个 structure 单元就是 DICOM Sequence 里的一个 Item。Item 的开头是一个特定 element (FFFE, E000)。 如果 Item 的长度是 -1， 要靠找到 (FFFE, E00D) 来决定 Item 的结束。 Pixel Data (7fe0, 0010) 是一个特殊的 DICOM element。总是在所有元素的最后面。它个 Sequence 有两个相似的地方： o 长度区总是 32-位 (即 explicit VR 的情况下要在 VR 区后面填两个 bytes，然后再加 4-bytes 的长度。 o 如图像是压缩的，每幅图用一个 item 来存。 第一个 item 是个 offset table。每幅图的 offset 是一个 dword (4 bytes)，第一幅图的 offset 是 0。

## DICOM 介质与 DICOMDIR： 

DICOM 图像通过 Store SCU/SCP 来传的时候最小的组是 (0008, xxxx)，一般头一个 element 是 (0008, 0000) 或 (0008, 0005)。 而存在介质上 （如光盘，硬盘和 MOD），则要加两套东西： 

1\. Preamble 和 DICOM signature: 128 bytes (一般是空的) 后面跟 DICM。 

2\. Meta header: group (0002, xxxx) 的十几个 elements. 看 DICOM Part 10。 DICOMDIR 是一个可变长度 迷你 database 文件。由 group (0002, xxxx) 和 group (0004, xxxx) 为主题。描述的是一个 4 层的树状结构 (tree structure)。 1. Patient 2. Study

3\. Series 4. Image 除了用 DICOM 惯用的 Sequene 外还用了 offset 来做 linked list。所以读写起来比较麻烦。请看 DICOM Part 10。

DICOM 的中文支持： DICOM 本身可以支持任何文字。应用程序则可能无法破译。 DICOM 图像用中文是可以通过下列方法实现： （0008，0005）设 "GB2312" 或 “Big5"。 所有文字用双字节 GB 或 Big 5 码。 大部分的 DICOM 控件用起来都会有问题。国标用的是 8-bit bytes 而非 7-bit printable ASCII。 ANSI 字串函数如 sprintf, sscanf 会将最高一个 bit 削掉。要改写成支持 wide character 的才行。