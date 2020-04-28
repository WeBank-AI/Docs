# 印尼 KTP OCR 请求接口文档

**版本记录**
 
| 版本 | 更新内容 |
| ---- | ---- |
| 20190819 | 初稿 |
| 20190902 | 更新请求样例，更新错误码 |
| 20190917 | 更新返回图片格式，更新错误码 |
| 20190918 | 更新错误信息字段 |

## 1. 接口信息

 * 地址: https://fis.webank.com/saas/ocr/ktp 
 * 类型: POST

## 2. 请求参数

请求包体为 JSON 格式。

### 2.1 参数列表

| 参数名称 | 参数类型 | 描述 | 默认值 |
| ---- | ---- | ---- | ---- |
| image | String`*` | 图片内容，**Base64 编码**后的 JPG 或 PNG 格式图片 | |
| session_id | String | 用户自定义的唯一会话id | 默认为空
| ret_image | Bool | 是否返回识别后的切图（切图是指精确剪裁对齐后的身份证正反面图片），返回格式为JPEG格式二进制图片使用base64编码后的字符串 | False |
| ret_portrait | Bool | 是否返回 KTP 中的人脸图片，返回格式为JPEG格式二进制图片使用base64编码后的字符串 | False |
| enable_border_check | Bool | 身份证遮挡检测开关，如果输入图片中的身份证卡片边框不完整则返回告警 | False |
| enable_detect_copy | Bool | 复印件检测开关，如果输入图片中的身份证卡片是复印件，则返回告警 | False |
| enable_quality_value | Bool | 是否返回图片质量数值（图片质量分数是评价一个图片的模糊程度的标准） | False |

* 带 `*` 号为必传参数

## 3. 返回结果

OCR 结果通过 HTTP Response 返回，HTTP Body 为 JSON 字符串，UTF-8 编码。

### 3.1 参数列表

| 一级 | 二级参数 | 参数类型 |  参数含义 | 字段样例 | 印尼文字段名 |
| ---- | ---- | ---- | ---- | ---- | ---- |
| errorcode | | Int | 错误码 | | |
| session_id | | String | 相应请求的session标识符 | |
| errormsg | | String | 错误信息（中文）| |
| warnmsg | | JSONArray | 多重告警码 | | | 
| ocr_result | | JSONObject | 文字识别结果 |  | |
| - | nik | String | 身份号码（16位） | "3275021310880010" | NIK |
| - | name | String | 姓名 | | Name |
| - | province | String | 地址-省份 | "JAWA TENGAH | Provinsi |
| - | city | String | 地址-市或县 | "KOTA SURAKARTA" | Kabupaten |
| - | alamat | String | 地址 |  | Alamat | 
| - | kecamatan | String | 地址-区 | | Alamat - Kecamatan |
| - | keldesa | String | 地址-社区/村 | | Alamat - Kel/DESA | 
| - | rtrw | String | 地址-街道 | 001/002 | Alamat - RW/RW |
| - | birthPlace | String | 出生地 | "SIGOTOM." | Tempat/Tgl Lahir |
| - | birthday | String | 生日 | "23-08-2019" | Tempat/Tgl Lahir | 
| - | validthru | String | 有效期至 | "11-01-2019", "SEUMUR HIDUP"(长期） | Berlaku Hingga |
| - | maritalStatus | String | 婚姻状况, Kawin-已婚,  | "Kawin" | Status Perkawinan |
| - | sex | String | 性别，男-"LAKI-LAKI", 女-"Perempuan" | "Perempuan" | Jenis Kelamin |
| - | bloodtype | String | 血型, "A"/"B"/"O"/"AB"/"-" | "-" | Gol. Darah |
| - | religion | String | 宗教: 伊斯兰教-"ISLAM",天主教-"KATOLIK",基督教-"KRISTEN",佛教-"BUDDHA",印度教-"HINDU",儒家-"KONGHUCU" | "KRISTEN" | Agama |
| - | citizenship | String | 国籍,印尼公民-"WNI"，外国人"WNA" | "WNI" | Kewarganegaraan |
| - | occupation | String | 职业 | "KARYAWAN SWASTA" | Pekerjaan |
| - | placeOfIssue | String | 签发地点 | "JAKARTA SELATAN" | Lokasi |
| - | dateOfIssue | String |签发日期 | "22-01-2016" | Waktu |
| image_result | | JSONObject | 图片检测结果 | | |
| - | ktp | String | KTP 区域图片，使用 Base64 编码后的字符串，是否返回由请求参数 ret_image 决定 | | |
| - | portrait | String | KTP 人像图片，使用 Base64 编码后的字符串，是否返回由请求参数 ret_portrait 决定 | | |
| - | ktp_bbox | JSONArray | KTP 框坐标，格式为 [[x0, y0], [x1, y1], [x2, y2], [x3, y3]] |
注：

1) 印尼行政区域划分方案：https://zh.wikipedia.org/wiki/印度尼西亚行政区划

划分级别（四级）：

* 省 - Provinsi

* 市/县 - Kabupaten

* 区 - Kecamatan

* 村 - Desa，社区 - Kelurahan

### 3.2 结果样例

```
{
    "errorcode": 0,
    "errormsg": "OK",
    "warnmsg": [],
    "session_id": "SESSION ID",
    "ocr_result": {
        "nik": "0123456789012345",
        "name": "Name",
        "province": "Province",
        "city": "City",
        "kecamatan": "Kecamtan",
        "keldesa": "Keldesa",
        "rtrw": "RTRW",
        "birthPlace": "birthPlace",
        "birthday": "10-15-2000",
        "maritalStatus": "MaritalStatus",
        "sex": "sex",
        "bloodtype": "Bloodtype",
        "religion": "Religion",
        "citizenship": "Citizenship",
        "occupation": "Occupation",
        "validthru" : "11-01-2019"
    }
}
```

### 3.2 错误码

| 错误码 | 错误信息 | 
| ---- | ---- |
| 0 | 识别正常 |
| 53090001 | 请求解析失败 |
| 53090002 | 图片解码错误 |
| 53090003 | OCR 内部错误 |
| 53090004 | 非KTP证件 |

### 3.3 告警码

| 告警码 | 含义 | 
| ---- | ---- |
| 53091001 | 黑白复印件 |
| 53091002 | 翻拍件 |
| 53091003 | 无法检测到人脸 |
| 53091004 | 证件信息不符 |
| 53091005 | 证件过期 |
