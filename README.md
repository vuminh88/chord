你好！
很冒昧用这样的方式来和你沟通，如有打扰请忽略我的提交哈。我是光年实验室（gnlab.com）的HR，在招Golang开发工程师，我们是一个技术型团队，技术氛围非常好。全职和兼职都可以，不过最好是全职，工作地点杭州。
我们公司是做流量增长的，Golang负责开发SAAS平台的应用，我们做的很多应用是全新的，工作非常有挑战也很有意思，是国内很多大厂的顾问。
如果有兴趣的话加我微信：13515810775  ，也可以访问 https://gnlab.com/，联系客服转发给HR。
# Chord
[WIP]
Implementation of Chord paper

# Paper
https://pdos.csail.mit.edu/papers/ton:chord/paper-ton.pdf

## Example Usage

```go
package main

import (
	"github.com/arriqaaq/chord"
	"github.com/arriqaaq/chord/internal"
	"log"
	"os"
	"os/signal"
	"time"
)

func createNode(id string, addr string, joinNode *internal.Node) (*chord.Node, error) {

	cnf := chord.DefaultConfig()
	cnf.Id = id
	cnf.Addr = addr
	cnf.Timeout = 10 * time.Millisecond
	cnf.MaxIdle = 100 * time.Millisecond

	n, err := chord.NewNode(cnf, joinNode)
	return n, err
}


func main() {

	joinNode := chord.NewInode("1", "0.0.0.0:8001")

	h, err := createNode("8", "0.0.0.0:8003", joinNode)
	if err != nil {
		log.Fatalln(err)
		return
	}

	c := make(chan os.Signal, 1)
	signal.Notify(c, os.Interrupt)
	<-c
	h.Stop()
}
```


# References
This implementation helped me a lot in designing the code base
https://github.com/r-medina/gmaj

# TODO
- Add more test cases
- Add stats/prometheus stats
