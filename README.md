```go
package main

import (
	"os"
	"strconv"

	"github.com/gofiber/fiber"
)

func main() {
	app := fiber.New()

	app.Get("/ip", func(ctx *fiber.Ctx) {
		if ctx.Get("X-Forwarded-For") != "" && ctx.Query("x-forwarded-for") != "false" {
			ctx.SendString(ctx.Get("X-Forwarded-For"))
			return
		}

		ctx.SendString(ctx.IP())
	})

	port := 8088
	var err error
	if os.Getenv("PORT") != "" {
		port, err = strconv.Atoi(os.Getenv("PORT"))
		if err != nil {
			panic(err)
		}
	}
	app.Listen(port)
}
```
