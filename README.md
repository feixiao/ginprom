我是光年实验室高级招聘经理。
我在github上访问了你的开源项目，你的代码超赞。你最近有没有在看工作机会，我们在招软件开发工程师，拉钩和BOSS等招聘网站也发布了相关岗位，有公司和职位的详细信息。
我们公司在杭州，业务主要做流量增长，是很多大型互联网公司的流量顾问。公司弹性工作制，福利齐全，发展潜力大，良好的办公环境和学习氛围。
公司官网是http://www.gnlab.com,公司地址是杭州市西湖区古墩路紫金广场B座，若你感兴趣，欢迎与我联系，
电话是0571-88839161，手机号：18668131388，微信号：echo 'bGhsaGxoMTEyNAo='|base64 -D ,静待佳音。如有打扰，还请见谅，祝生活愉快工作顺利。

<h1 align="center">📡 ginprom</h1>
<p align="center">
    <em>Prometheus metrics exporter for Gin.Inspired by <a href="https://github.com/Depado/ginprom">Depado/ginprom.</a></em>
</p>

### 🔰 Installation

```shell
$ go get -u github.com/feixiao/ginprom
```

### 📝 Usage

It's easy to get started with ginprom, only a few lines of code needed.

```golang
import (
	"github.com/feixiao/ginprom"
	"github.com/gin-gonic/gin"
	"github.com/prometheus/client_golang/prometheus/promhttp"
)

func main() {
    r := gin.Default()
	// use prometheus metrics exporter middleware.
	//
	// ginprom.PromMiddleware() expects a ginprom.PromOpts{} poniter.
	// It was used for filtering labels with regex. `nil` will pass every requests.
	//
	// ginprom promethues-labels: 
	//   `status`, `endpoint`, `method`
	//
	// for example:
	// 1). I want not to record the 404 status request. That's easy for it.
	// ginprom.PromMiddleware(&ginprom.PromOpts{ExcludeRegexStatus: "404"})
	//
	// 2). And I wish ignore endpoint start with `/prefix`.
	// ginprom.PromMiddleware(&ginprom.PromOpts{ExcludeRegexEndpoint: "^/prefix"})
	r.Use(ginprom.PromMiddleware(nil))

    // register the `/metrics` route.
	r.GET("/metrics", ginprom.PromHandler(promhttp.Handler()))

    // your working routes
	r.GET("/", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{"message": "home"})
    })
}
```

### 🎉 Metrics

Details about exposed Prometheus metrics.

| Name | Type | Exposed Information |
| ---- | ---- | ---------------------|
| service_uptime						| Counter	| HTTP service uptime. |
| service_http_request_count_total		| Counter	| Total number of HTTP requests made. |
| service_http_request_duration_seconds | Histogram | HTTP request latencies in seconds. |
| service_http_request_size_bytes 		| Summary	| HTTP request sizes in bytes. |
| service_http_response_size_bytes 		| Summary	|HTTP request sizes in bytes. |


### 📊 Grafana

Although Promethues offers a simple dashboard, Grafana is clearly a better choice. [Grafana configuration](./ginprom-service.json).

![](https://user-images.githubusercontent.com/19553554/65812184-19a5a000-e1f6-11e9-8881-e0c260196bc9.png)


### 📃 LICENSE

MIT [©chenjiandongx](https://github.com/chenjiandongx)
