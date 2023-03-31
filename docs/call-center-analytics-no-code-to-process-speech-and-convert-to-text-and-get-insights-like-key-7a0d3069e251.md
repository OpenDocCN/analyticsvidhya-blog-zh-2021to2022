# 呼叫中心分析—无需代码即可处理语音并转换为文本，从而获得关键短语、PII、情感和实体等洞察

> 原文：<https://medium.com/analytics-vidhya/call-center-analytics-no-code-to-process-speech-and-convert-to-text-and-get-insights-like-key-7a0d3069e251?source=collection_archive---------5----------------------->

# 使用 Azure 认知服务语音转文本和逻辑应用

# 无代码—工作流风格

请以此为参考，因为这可能不是您的确切使用案例。但你可以看到人工智能如何帮助推动见解和自动化大型音频数据集

# 先决条件

*   Azure 帐户
*   Azure 存储帐户
*   Azure 认知服务
*   Azure 逻辑应用
*   获取存储的连接字符串
*   获取主键以用作认知服务的订阅键
*   音频文件应该是 wav 格式
*   音频文件不能太大
*   音频时间 10 分钟

# 体系结构

![](img/8162a4f51337f5e3ccd9c218cca6573f.png)

# 逻辑应用

*   首先从 Blob 创建一个触发器

![](img/830cd017d0c1476e4ee2727919a18e5d.png)

*   使用 blob 连接字符串创建连接字符串

![](img/10d1e37abe18216b9a2c259114fc675b.png)

*   现在带来“从 Azure 存储读取 Blob 内容”

![](img/6e78445d3372068db324373f87d96bf9.png)

*   容器名称:音频输入
*   选择动态，并选择如上图的斑点名称
*   带来 HTTP 操作
*   这里我们需要调用语音到文本并传递参数
*   接受:application/JSON；文本/xml
*   内容类型:音频/wav；编解码器=音频/PCM；采样率=16000
*   预期:100-继续
*   OCP-Apim-订阅-密钥:xxxx-xxxxxx-xxxxxx-xxxx
*   传输编码:分块

![](img/b0facab3e184ec982fd55f5b8c87d2ad.png)

*   对于正文，选择读取 blob 内容
*   这应该将音频二进制内容传递给认知服务 api
*   现在让我们解析 api 输出
*   现在从 http 输出中选择主体
*   提供架构作为

```
{
    "properties": {
        "Duration": {
            "type": "integer"
        },
        "NBest": {
            "items": {
                "properties": {
                    "Confidence": {
                        "type": "number"
                    },
                    "Display": {
                        "type": "string"
                    },
                    "ITN": {
                        "type": "string"
                    },
                    "Lexical": {
                        "type": "string"
                    },
                    "MaskedITN": {
                        "type": "string"
                    }
                },
                "required": [
                    "Confidence",
                    "Lexical",
                    "ITN",
                    "MaskedITN",
                    "Display"
                ],
                "type": "object"
            },
            "type": "array"
        },
        "Offset": {
            "type": "integer"
        },
        "RecognitionStatus": {
            "type": "string"
        }
    },
    "type": "object"
}
```

![](img/b10b42d41b2784c4d8f6ac0857715563.png)

*   现在添加将数据上传到 blob 的操作
*   给出一个容器输出
*   给出一个输出名称

![](img/9a9240661b3b4f4791e968dcfc50a32d.png)

*   转到概述，然后单击运行触发器，然后单击->运行
*   上传 wav 文件
*   等待它处理

![](img/f3d7c5886aa632e0137debd79e99fc76.png)

*   给语音 API 一些时间来处理
*   现在转到 blob 存储

```
{
  "RecognitionStatus": "Success",
  "Offset": 300000,
  "Duration": 524000000,
  "NBest": [
    {
      "Confidence": 0.972784698009491,
      "Lexical": "the speech SDK exposes many features from the speech service but not all of them the capabilities of the speech SDK are often associated with scenarios the speech SDK is ideal for both real time and non real time scenarios using local devices files azure blob storage and even input and output streams when a scenario is not achievable with the speech SDK look for a rest API alternative speech to text also known as speech recognition transcribes audio streams to text that your applications tools or devices can consume more display use speech to text with language understanding louis to deride user intents from transcribed speech and act on voice commands you speech translation to translate speech input to a different language with a single call for more information see speech to text basics",
      "ITN": "the speech SDK exposes many features from the speech service but not all of them the capabilities of the speech SDK are often associated with scenarios the speech SDK is ideal for both real time and non real time scenarios using local devices files azure blob storage and even input and output streams when a scenario is not achievable with the speech SDK look for a rest API alternative speech to text also known as speech recognition transcribes audio streams to text that your applications tools or devices can consume more display use speech to text with language understanding louis to deride user intents from transcribed speech and act on voice commands you speech translation to translate speech input to a different language with a single call for more information see speech to text basics",
      "MaskedITN": "the speech sdk exposes many features from the speech service but not all of them the capabilities of the speech sdk are often associated with scenarios the speech sdk is ideal for both real time and non real time scenarios using local devices files azure blob storage and even input and output streams when a scenario is not achievable with the speech sdk look for a rest api alternative speech to text also known as speech recognition transcribes audio streams to text that your applications tools or devices can consume more display use speech to text with language understanding louis to deride user intents from transcribed speech and act on voice commands you speech translation to translate speech input to a different language with a single call for more information see speech to text basics",
      "Display": "The Speech SDK exposes many features from the speech service, but not all of them. The capabilities of the speech SDK are often associated with scenarios. The Speech SDK is ideal for both real time and non real time scenarios using local devices files, Azure blob storage and even input and output streams. When a scenario is not achievable with the speech SDK, look for a rest API. Alternative speech to text, also known as speech recognition, transcribes audio streams to text that your applications, tools or devices can consume more display use speech to text with language, understanding Louis to deride user intents from transcribed speech and act on voice commands. You speech translation to translate speech input to a different language with a single call. For more information, see speech to text basics."
    }
  ]
}
```

*   以上是示例输出
*   置信度得分和显示可用
*   现在处理文本分析和拉关键短语，PII，情绪和实体
*   创建 3 个变量，一个用于 id、文本和语言
*   创建 id

![](img/1190032e3024730c593a28158a81175d.png)

*   创造语言

![](img/3dc463b113edf08d06cae1cfbed0c175.png)

*   创建文本

![](img/16a839f0f2b4010c18025c764ae8b44c.png)

*   接下来是作曲

![](img/29bddfc877c809f5d21b7bd04d7f3187.png)

```
{
  "documents": [
    {
      "id": @{variables('id')},
      "language": @{variables('language')},
      "text": @{variables('text')}
    }
  ]
}
```

*   文本分析 API

```
https://cogsvcname.cognitiveservices.azure.com/text/analytics/v3.1/keyPhrases
```

*   提供 Header-Ocp-Apim-Subscription-Key
*   标题-内容-类型
*   来自合成输出的正文内容

![](img/bca662ba60bb53c9d0d4618642b25d0d.png)

*   解析 JSON 输出

![](img/802dd341bbddc06907a3d63271922d03.png)

*   (计划或理论的)纲要

```
{
    "properties": {
        "documents": {
            "items": {
                "properties": {
                    "id": {
                        "type": "string"
                    },
                    "keyPhrases": {
                        "items": {
                            "type": "string"
                        },
                        "type": "array"
                    },
                    "warnings": {
                        "type": "array"
                    }
                },
                "required": [
                    "id",
                    "keyPhrases",
                    "warnings"
                ],
                "type": "object"
            },
            "type": "array"
        },
        "errors": {
            "type": "array"
        },
        "modelVersion": {
            "type": "string"
        }
    },
    "type": "object"
}
```

*   删除 blob
*   blob 的名称:textanalytics.json

![](img/30b9f14a4c7b3f187a787a15cfb7c63b.png)

*   现在保存 blob
*   blob 的名称:textanalytics.json

![](img/87b5de07d784ce2ecc09e15322242d17.png)

*   现在给 PII 的文本分析打电话

![](img/4d33e7f21a1849c62972210fbf0c5ffc.png)

```
[https://cogsvcnmae.cognitiveservices.azure.com/text/analytics/v3.1/entities/recognition/pii](https://cogsvcnmae.cognitiveservices.azure.com/text/analytics/v3.1/entities/recognition/pii)
```

*   提供标题-Ocp-Apim-订阅-密钥
*   标题—内容类型
*   正文—合成输出中的内容
*   现在带上帕西森

![](img/f717b6edf966bd1e72478895bb1d815c.png)

```
{
    "type": "object",
    "properties": {
        "documents": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "redactedText": {
                        "type": "string"
                    },
                    "id": {
                        "type": "string"
                    },
                    "entities": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "properties": {
                                "text": {
                                    "type": "string"
                                },
                                "category": {
                                    "type": "string"
                                },
                                "offset": {
                                    "type": "integer"
                                },
                                "length": {
                                    "type": "integer"
                                },
                                "confidenceScore": {
                                    "type": "number"
                                }
                            },
                            "required": [
                                "text",
                                "category",
                                "offset",
                                "length",
                                "confidenceScore"
                            ]
                        }
                    },
                    "warnings": {
                        "type": "array"
                    }
                },
                "required": [
                    "redactedText",
                    "id",
                    "entities",
                    "warnings"
                ]
            }
        },
        "errors": {
            "type": "array"
        },
        "modelVersion": {
            "type": "string"
        }
    }
}
```

*   现在请删除
*   blob 名称:textpii.json

![](img/c9e0956ffabe080886008dbc91433b84.png)

*   现在将文件保存到 blob 中
*   blob 名称:textpii.json

![](img/a7e5c6862c9b555472d2d01a1be1f7dc.png)

*   现在获取情绪 API

![](img/0686dc40084d0d2ae9b20b3902ece581.png)

```
[https://cogsvcnmae.cognitiveservices.azure.com/text/analytics/v3.1/sentiment](https://cogsvcnmae.cognitiveservices.azure.com/text/analytics/v3.1/sentiment)
```

*   提供标题-Ocp-Apim-订阅-密钥
*   标题—内容类型
*   正文—合成输出中的内容
*   带上帕西森

![](img/b9aba79b3ff8ef99e232d04a2aa22ab6.png)

```
{
    "type": "object",
    "properties": {
        "documents": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "string"
                    },
                    "sentiment": {
                        "type": "string"
                    },
                    "confidenceScores": {
                        "type": "object",
                        "properties": {
                            "positive": {
                                "type": "number"
                            },
                            "neutral": {
                                "type": "number"
                            },
                            "negative": {
                                "type": "number"
                            }
                        }
                    },
                    "sentences": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "properties": {
                                "sentiment": {
                                    "type": "string"
                                },
                                "confidenceScores": {
                                    "type": "object",
                                    "properties": {
                                        "positive": {
                                            "type": "number"
                                        },
                                        "neutral": {
                                            "type": "number"
                                        },
                                        "negative": {
                                            "type": "number"
                                        }
                                    }
                                },
                                "offset": {
                                    "type": "integer"
                                },
                                "length": {
                                    "type": "integer"
                                },
                                "text": {
                                    "type": "string"
                                }
                            },
                            "required": [
                                "sentiment",
                                "confidenceScores",
                                "offset",
                                "length",
                                "text"
                            ]
                        }
                    },
                    "warnings": {
                        "type": "array"
                    }
                },
                "required": [
                    "id",
                    "sentiment",
                    "confidenceScores",
                    "sentences",
                    "warnings"
                ]
            }
        },
        "errors": {
            "type": "array"
        },
        "modelVersion": {
            "type": "string"
        }
    }
}
```

*   现在请删除
*   blob 名称:text perspection . JSON

![](img/872c4940d42b88c1472709982edf58f8.png)

*   现在带来保存 blob
*   blob 名称:text perspection . JSON

![](img/0c10d2cce7d228ebe30681c23a7fae89.png)

*   现在获取实体

![](img/a8ef355def1d47a37d159db8cc14a3e6.png)

```
[https://cogsvcnmae.cognitiveservices.azure.com/text/analytics/v3.1/entities/recognition/general](https://cogsvcnmae.cognitiveservices.azure.com/text/analytics/v3.1/entities/recognition/general)
```

*   提供标题-Ocp-Apim-订阅-密钥
*   标题—内容类型
*   正文—合成输出中的内容
*   带上帕西森

![](img/baff007646a9bd906eff8edabb0d39cc.png)

```
{
    "type": "object",
    "properties": {
        "documents": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "string"
                    },
                    "entities": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "properties": {
                                "text": {
                                    "type": "string"
                                },
                                "category": {
                                    "type": "string"
                                },
                                "subcategory": {
                                    "type": "string"
                                },
                                "offset": {
                                    "type": "integer"
                                },
                                "length": {
                                    "type": "integer"
                                },
                                "confidenceScore": {
                                    "type": "number"
                                }
                            },
                            "required": [
                                "text",
                                "category",
                                "offset",
                                "length",
                                "confidenceScore"
                            ]
                        }
                    },
                    "warnings": {
                        "type": "array"
                    }
                },
                "required": [
                    "id",
                    "entities",
                    "warnings"
                ]
            }
        },
        "errors": {
            "type": "array"
        },
        "modelVersion": {
            "type": "string"
        }
    }
}
```

*   现在请删除
*   blob 名称:textentities.json

![](img/4c625734b5d3286dae2e4ad973618554.png)

*   现在保存最终输出

![](img/16e07a744a6547b8a1111d52c827a695.png)

原文位于— [Samples2021/audiotext.md 位于主 balakreshnan/samples 2021(github.com)](https://github.com/balakreshnan/Samples2021/blob/main/AzureAI/audiotext.md)