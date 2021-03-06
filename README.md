# zcfg
[![Build Status](https://travis-ci.org/archdx/zcfg.svg?branch=master)](https://travis-ci.org/archdx/zcfg)
[![codecov](https://codecov.io/gh/archdx/zcfg/branch/master/graph/badge.svg)](https://codecov.io/gh/archdx/zcfg)

### Example
```go
type Config struct {
	API        *APIConfig        `yaml:"api"        flag:"api"`
	Clickhouse *ClickhouseConfig `yaml:"clickhouse" flag:"clickhouse"`
	Log        *LogConfig        `yaml:"log"        flag:"log"`
}

type APIConfig struct {
	Port uint16 `yaml:"port" flag:"port" env:"API_PORT"`
}

type ClickhouseConfig struct {
	Host        string        `yaml:"host"        flag:"host"        env:"CLICKHOUSE_HOST"`
	User        string        `yaml:"user"        flag:"user"        env:"CLICKHOUSE_USER"`
	Database    string        `yaml:"database"    flag:"database"    env:"CLICKHOUSE_DATABASE"`
	ReadTimeout time.Duration `yaml:"readTimeout" flag:"readTimeout" env:"CLICKHOUSE_READ_TIMEOUT"`
}

type LogConfig struct {
	Level string `yaml:"level" flag:"level" env:"LOG_LEVEL"`
}

func main() {
	var cfg Config

	cfgLoader := zcfg.New(&cfg, zcfg.FromFile("config.yaml"), zcfg.UseFlags(flag.CommandLine))
	if err := cfgLoader.Load(); err != nil {
		log.Fatal(err)
	}
}
```
```bash
LOG_LEVEL=DEBUG go run main.go -c config.yaml --api.port=8001 --clickhouse.user=testuser --clickhouse.database=testdb --clickhouse.readTimeout=1s
```

### TODO
- vault support for passwords
