SERVICE_NAME ?= example
DOCKERFILE := Dockerfile

.PHONY: build
build:
	docker build --progress=plain \
		--tag valgrind-demo \
		-f $(DOCKERFILE) .

.PHONY: run
run: 
	docker run valgrind-demo