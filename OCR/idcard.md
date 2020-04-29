# 身份证 OCR 请求接口文档

**版本记录**
 
| 版本 | 更新内容 |
| ---- | ---- |
| 20200423 | 初稿 |

## 1. 接口信息

 * 接口: https://fis.webank.com/saas/ocr/idcard
 * 类型: POST

## 2. 请求参数

请求包体为 JSON 格式。

### 2.1 参数列表

| 参数名称 | 参数类型 | 描述 | 默认值 |
| ---- | ---- | ---- | ---- |
| image | String`*` | 图片内容，**Base64 编码**后的 JPG 或 PNG 格式图片 | |
| session_id | String`*` | 用户自定义的唯一会话id | |
| ret_image | Bool | 是否返回识别后的切图（切图是指精确剪裁对齐后的身份证正反面图片），返回格式为JPEG格式二进制图片使用base64编码后的字符串 | False |
| ret_portrait | Bool | 是否返回身份证（人像面）的人脸图片，返回格式为JPEG格式二进制图片使用base64编码后的字符串 | False |
| ref_side | String | 当图片中同时存在身份证正反面时，通过该参数指定识别的版面：取值  'Any' - 识别人像面或国徽面，'F' - 仅识别人像面，'B' - 仅识别国徽面 |  'Any' |
| enable_border_check | Bool | 身份证遮挡检测开关，如果输入图片中的身份证卡片边框不完整则返回告警 | False |
| enable_detect_copy | Bool | 复印件、翻拍件检测开关，如果输入图片中的身份证卡片是复印件，则返回告警 | False |
| enable_quality_value | Bool | 是否返回图片质量数值（图片质量分数是评价一个图片的模糊程度的标准） | False |

* 带 `*` 号为必传参数

## 3. 返回结果

OCR 结果通过 HTTP Response 返回，HTTP Body 为 JSON 字符串，UTF-8 编码。HTTP 返回状态码请参考下文错误码。

### 3.1 参数列表

| 一级 | 二级参数 | 参数类型 |  参数含义 | 字段样例 |
| ---- | ---- | ---- | ---- | ---- |
| errorcode | | Int | 错误码 | |
| session_id | | String | 相应请求的session标识符 |
| errormsg | | String | 错误信息（中文）|
| warnmsg | | JSONArray | 多重告警码 | | 
| ocr_result | | JSONObject | 文字识别结果 |  | |
| - | side | String | F-身份证人像面，B-身份证国徽面 | 
| - | idno | String | 身份号码（人像面） | |
| - | name | String | 姓名（人像面） | |
| - | nation | String | 民族（人像面） | |
| - | gender | String | 性别（人像面） | |
| - | address | String | 地址（人像面） | |
| - | birthdate | String | 生日（人像面） | 19900111 |
| - | validthru | String | 有效期（国徽面） | 20000101-20100101 |
| - | issuedby | String | 签发机关（国徽面） | |
| image_result | | JSONObject | 图片检测结果 | |
| - | idcard | String | 身份证区域图片，使用 Base64 编码后的字符串，是否返回由请求参数 ret_image 决定 | |
| - | portrait | String | 身份证人像照片，使用 Base64 编码后的字符串，是否返回由请求参数 ret_portrait 决定 | |
| - | idcard_bbox | JSONArray | KTP 框坐标，格式为 `[[x0, y0], [x1, y1], [x2, y2], [x3, y3]]` |

### 3.2 结果样例

人像面：

```
Result:  {
    "errorcode": 0,
    "warnmsg": [],
    "ocr_result": {
        "side": "F",
        "idno": "440100201412151022",
        "name": "王微众",
        "nation": "汉",
        "gender": "男",
        "address": "广东省深圳市南山区沙河西路1819号",
        "birthdate": "20141215"
    },
    "image_result": {
        "idcard_bbox": [
            [35, 1079],
            [35, 0],
            [1791, 0],
            [1791, 1079]
        ]
    }
}
```

国徽面：

```
{
    "errorcode": 0,
    "warnmsg": [],
    "ocr_result": {
        "side": "B",
        "validthru": "20141215-20341215",
        "issuedby": "深圳公安局南山分局"
    },
    "image_result": {
        "idcard_bbox": [
            [12, 345],
            [12, 28],
            [546, 28],
            [546, 345]
        ]
    }
}
```

### 3.2 错误码

| 错误码 | 错误信息 | HTTP 状态码 |
| ---- | ---- | ---- |
| 0 | 识别正常 | 200 |
| 53090001 | 请求解析失败 | 400 |
| 53090002 | 图片解码错误 | 400 | 
| 53090003 | OCR 内部错误 | 500 |
| 53090004 | 无法识别的身份证 | 200 |

### 3.3 告警码

| 告警码 | 含义 | 
| ---- | ---- |
| 53091001 | 黑白复印件 |
| 53091002 | 翻拍件 |
| 53091003 | 无法检测到人脸 |
| 53091004 | 证件信息缺失或错误 |
| 53091005 | 证件过期 |
| 53091006 | 身份证不完整 | 
