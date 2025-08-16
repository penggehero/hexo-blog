---
title: golang 快速入门 MCP(大模型上下文协议)
date: 2025-08-07 18:42:44
tags:
  - golang
  - mcp
typora-root-url: ./golang-快速入门-MCP-大模型上下文协议
---

[TOC]

# 1.MCP 模型上下文协议 简介

[mcp官网]: https://modelcontextprotocol.io

MCP 是一个开放协议，它标准化了应用程序向大型语言模型 (LLM) 提供上下文的方式。MCP 就像 AI 应用程序的 USB-C 端口一样。正如 USB-C 提供了一种标准化的方式将您的设备连接到各种外围设备和配件一样，MCP 也提供了一种标准化的方式将 AI 模型连接到不同的数据源和工具。MCP 使您能够在 LLM 之上构建代理和复杂的工作流，并将您的模型与世界连接起来。

![img](model-context-protocol-architecture.webp)

MCP 提供：

- **越来越多的预建集成**可供您的 LLM 直接插入
- 为 AI 应用程序构建自定义集成**标准化方法**
- 每个人都可以自由实施和使用的**开放协议**
- 在不同应用程序之间切换并随身携带上下文的**灵活性**



# 2.go语言搭建获取城市天气的mcp server

整体流程如图所示:

```mermaid
sequenceDiagram
    participant 用户
    participant LLM
    participant 城市编码mcp as 城市编码mcp
    participant 天气mcp as 天气mcp

    用户->>LLM: 提问城市的天气
    LLM->>城市编码mcp: 调用获取城市编码
    LLM->>天气mcp: 调用获取城市天气数据
    LLM->>用户: 输出城市的天气情况

```

![image-20250807204019777](image-20250807204019777.png)


## 2.1 获取城市天气的api 接口

这里选取的高德平台的免费获取天气的api

api地址：https://amap.apifox.cn/api-14675765

![image-20250807185716460](image-20250807185716460.png)

代码如下：

```go
package main

import (
	"fmt"
	"log"

	"resty.dev/v3"
)

const (
	weatherInfoUrl = "https://restapi.amap.com/v3/weather/weatherInfo"
)

func getWeatherInfo(cityCode string) (string, error) {
	client := resty.New()
	defer client.Close()

	res, err := client.R().
		EnableTrace().
		Get(fmt.Sprintf("%s?city=%s&key=%s", weatherInfoUrl, cityCode, key))
	if err != nil {
		log.Println(err)
		return "", err
	}
	return res.String(), nil
}

```

这里api 接口传递的是城市编码，不是直接传入的城市名称，所以还需要城市名称转城市编码的接口。

## 2.2 行政区域查询的接口

api地址： https://lbs.amap.com/api/webservice/guide/api/district

![image-20250807191006828](image-20250807191006828.png)

代码：

```go
package main

import (
	"encoding/json"
	"fmt"
	"log"

	"resty.dev/v3"
)

const (
	districtUrl = "https://restapi.amap.com/v3/config/district?keywords=%s&subdistrict=2&key=%s"
)

func getDistrict(str string) ([]string, error) {
	client := resty.New()
	defer client.Close()
	res, err := client.R().
		EnableTrace().
		Get(fmt.Sprintf(districtUrl, str, key))
	if err != nil {
		log.Println(err)
		return nil, err
	}
	var resp Response
	err = json.Unmarshal(res.Bytes(), &resp)
	codes, err := GetCityCodes(res.Bytes())
	if err != nil {
		fmt.Println(err)
		return nil, err
	}
	return codes, nil
}

// Response struct
type Response struct {
	Status     string `json:"status"`
	Info       string `json:"info"`
	Infocode   string `json:"infocode"`
	Count      string `json:"count"`
	Suggestion struct {
		Keywords []interface{} `json:"keywords"`
		Cities   []interface{} `json:"cities"`
	} `json:"suggestion"`
	Districts []struct {
		Citycode  string `json:"citycode"`
		Adcode    string `json:"adcode"`
		Name      string `json:"name"`
		Center    string `json:"center"`
		Level     string `json:"level"`
		Districts []struct {
			Citycode  string `json:"citycode"`
			Adcode    string `json:"adcode"`
			Name      string `json:"name"`
			Center    string `json:"center"`
			Level     string `json:"level"`
			Districts []struct {
				Citycode  string `json:"citycode"`
				Adcode    string `json:"adcode"`
				Name      string `json:"name"`
				Center    string `json:"center"`
				Level     string `json:"level"`
				Districts []struct {
					Citycode  string        `json:"citycode"`
					Adcode    string        `json:"adcode"`
					Name      string        `json:"name"`
					Center    string        `json:"center"`
					Level     string        `json:"level"`
					Districts []interface{} `json:"districts"`
				} `json:"districts"`
			} `json:"districts"`
		} `json:"districts"`
	} `json:"districts"`
}

// GetCityCodes 获取城市编码
func GetCityCodes(jsonData []byte) ([]string, error) {
	var resp Response
	err := json.Unmarshal(jsonData, &resp)
	if err != nil {
		return nil, err
	}
	fmt.Println(resp)

	var cityCodes []string
	for _, district := range resp.Districts {
		if district.Adcode != "" {
			cityCodes = append(cityCodes, district.Adcode)
		}
		for _, subDistrict := range district.Districts {
			if subDistrict.Adcode != "" {
				cityCodes = append(cityCodes, subDistrict.Adcode)
			}
			for _, subSubDistrict := range subDistrict.Districts {
				if subSubDistrict.Adcode != "" {
					cityCodes = append(cityCodes, subSubDistrict.Adcode)
				}
				for _, street := range subSubDistrict.Districts {
					if street.Adcode != "" {
						cityCodes = append(cityCodes, street.Adcode)
					}
				}
			}
		}
	}

	// 去重
	uniqueCodes := make(map[string]bool)
	var result []string
	for _, code := range cityCodes {
		if !uniqueCodes[code] {
			uniqueCodes[code] = true
			result = append(result, code)
		}
	}

	return result, nil
}

```

因为城市存在多个区划，所以这里返回一个城市编码列表。

### 2.3 搭建mcp sse sever

mcp 有多种传输模式：

- STDIO 传输：STDIO 传输在本地机器上运行，并通过标准输入/输出流进行通信。
- SSE 传输：服务器发送事件 (SSE) 传输在远程服务器上运行，并通过 [HTTP/HTTPS](https://zhida.zhihu.com/search?content_id=256005974&content_type=Article&match_order=1&q=HTTP%2FHTTPS&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NTQ3MzgzMjUsInEiOiJIVFRQL0hUVFBTIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjU2MDA1OTc0LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.QQSCi5WPrAbIk8H4Y5E1jSJW5EGwW5agYnAPZBsS7ro&zhida_source=entity) 进行通信。

可参考了解下面的文章：

https://modelcontextprotocol.io/specification/2025-06-18/basic/transports#custom-transports

https://modelcontextprotocol.io/specification/2024-11-05/basic/transports#stdio



代码如下：

```go
package main

import (
	"context"
	"fmt"
	"log"

	"github.com/mark3labs/mcp-go/mcp"
	"github.com/mark3labs/mcp-go/server"
)

const (
	Port = ":8080"
)

func main() {
	// 创建Mcp server
	s := server.NewMCPServer(
		"获取城市天气信息",
		"1.0.0",
		server.WithToolCapabilities(false),
		server.WithRecovery(),
	)

	// 定义获取城市编码工具
	getDistrictTool := mcp.NewTool("getDistrict",
		mcp.WithDescription("获取城市代码，输入城市名，返回城市代码列表"),
		mcp.WithString("keyword",
			mcp.Required(),
			mcp.Description("输入城市名"),
		),
	)

	// 定义获取城市天气工具
	getWeatherInfoTool := mcp.NewTool("getWeatherInfo",
		mcp.WithDescription("获取城市天气信息，输入城市代码，返回天气信息"),
		mcp.WithString("city_code",
			mcp.Required(),
			mcp.Description("输入城市代码"),
		),
	)

	// 添加工具的处理函数
	s.AddTool(getDistrictTool, func(ctx context.Context, request mcp.CallToolRequest) (*mcp.CallToolResult, error) {
		// Using helper functions for type-safe argument access
		keyword, err := request.RequireString("keyword")
		if err != nil {
			return mcp.NewToolResultError(err.Error()), nil
		}
		result, err := getDistrict(keyword)
		if err != nil {
			return mcp.NewToolResultError(err.Error()), nil
		}
		return mcp.NewToolResultText(fmt.Sprintf("%v", result)), nil
	})

	// 添加工具的处理函数
	s.AddTool(getWeatherInfoTool, func(ctx context.Context, request mcp.CallToolRequest) (*mcp.CallToolResult, error) {
		// Using helper functions for type-safe argument access
		cityCode, err := request.RequireString("city_code")
		if err != nil {
			return mcp.NewToolResultError(err.Error()), nil
		}
		result, err := getWeatherInfo(cityCode)
		if err != nil {
			return mcp.NewToolResultError(err.Error()), nil
		}
		return mcp.NewToolResultText(fmt.Sprintf("%v", result)), nil
	})

	// 创建一个SSE 模块的server
	sseServer := server.NewSSEServer(s)

	log.Printf("SSE server listening on port %s", Port)
	if err := sseServer.Start(Port); err != nil {
		log.Fatal(err)
	}
}

```

启动服务

```sh
 go build -o mcp-sever
 ./mcp-sever
 2025/08/07 19:24:46 SSE server listening on port :8080
```



# 3.在Cherry Studio 工具中使用mcp

## 3.1 配置

操作流程可参考官网：

https://docs.cherry-ai.com/advanced-basic/mcp/config

具体流程如下：

1. 打开 Cherry Studio 设置。
2. 找到 `MCP 服务器` 选项。
3. 点击 `添加服务器`。
4. 将 MCP Server 的相关参数填入（[参考链接](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch)）。可能需要填写的内容包括：
    - 名称：自定义一个名称
    - 类型：选择SSE模式
    - URL：填SSE的服务地址（后缀为/sse）
5. 点击 `保存`。

配置如图：

![image-20250807192525507](image-20250807192525507.png)

![image-20250807192746064](image-20250807192746064.png)

## 3.2 使用MCP工具

首先在对话下面开启MCP 工具，模型推荐使用chatgpt的

![image-20250807192907998](image-20250807192907998.png)

提问：

![企业微信截图_20250807170854](企业微信截图_20250807170854.png)

可以看到，大模型确实调用了刚刚创建的MCP 工具，并得到了正确结果！
