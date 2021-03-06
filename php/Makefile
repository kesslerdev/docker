TAG=$(PHP_VERSION)-$(PHP_VARIANT)
CUST_TAG=$(shell echo $(TAG) | sed -e 's/^\([0-9]*\.[0-9]*\)\.[0-9]*/\1/g')

TAGS_TO_PULL=$(TAG) $(TAG)-nodejs $(TAG)-tideways $(TAG)-nodejs-tideways
include ../Makefile

APCUBC_VERSION?=1.0.4
MEMCACHED_VERSION?=3.0.4
REDIS_VERSION?=3.1.6
REDIS_NEXT_VERSION?=4.2.0
COMPOSER_VERSION?=1.7.2
PRESTISSIMO_VERSION?=0.3.8

build:
	@docker build --quiet --rm \
		--build-arg IMAGE_NAME=$(IMAGE_NAME) \
		--build-arg PHP_VERSION=$(PHP_VERSION) \
		--build-arg PHP_VARIANT=$(PHP_VARIANT) \
		--build-arg APCUBC_VERSION=$(APCUBC_VERSION) \
		--build-arg MEMCACHED_VERSION=$(MEMCACHED_VERSION) \
		--build-arg REDIS_VERSION=$(REDIS_VERSION) \
		--build-arg REDIS_NEXT_VERSION=$(REDIS_NEXT_VERSION) \
		--build-arg COMPOSER_VERSION=$(COMPOSER_VERSION) \
		--build-arg PRESTISSIMO_VERSION=$(PRESTISSIMO_VERSION) \
		--target base \
		-t $(DOCKER_IMAGE_NAME):$(TAG) .
	@docker build --quiet --rm \
		--build-arg IMAGE_NAME=$(IMAGE_NAME) \
		--build-arg PHP_VERSION=$(PHP_VERSION) \
		--build-arg PHP_VARIANT=$(PHP_VARIANT) \
		--build-arg APCUBC_VERSION=$(APCUBC_VERSION) \
		--build-arg MEMCACHED_VERSION=$(MEMCACHED_VERSION) \
		--build-arg REDIS_VERSION=$(REDIS_VERSION) \
		--build-arg REDIS_NEXT_VERSION=$(REDIS_NEXT_VERSION) \
		--build-arg COMPOSER_VERSION=$(COMPOSER_VERSION) \
		--build-arg PRESTISSIMO_VERSION=$(PRESTISSIMO_VERSION) \
		--target nodejs \
		-t $(DOCKER_IMAGE_NAME):$(TAG)-nodejs .
	@docker build --quiet --rm \
		--build-arg IMAGE_NAME=$(IMAGE_NAME) \
		--build-arg PHP_VERSION=$(PHP_VERSION) \
		--build-arg PHP_VARIANT=$(PHP_VARIANT) \
		--build-arg APCUBC_VERSION=$(APCUBC_VERSION) \
		--build-arg MEMCACHED_VERSION=$(MEMCACHED_VERSION) \
		--build-arg REDIS_VERSION=$(REDIS_VERSION) \
		--build-arg REDIS_NEXT_VERSION=$(REDIS_NEXT_VERSION) \
		--build-arg COMPOSER_VERSION=$(COMPOSER_VERSION) \
		--build-arg PRESTISSIMO_VERSION=$(PRESTISSIMO_VERSION) \
		--target tideways \
		-t $(DOCKER_IMAGE_NAME):$(TAG)-tideways .
	@docker build --quiet --rm \
		--build-arg IMAGE_NAME=$(IMAGE_NAME) \
		--build-arg PHP_VERSION=$(PHP_VERSION) \
		--build-arg PHP_VARIANT=$(PHP_VARIANT) \
		--build-arg APCUBC_VERSION=$(APCUBC_VERSION) \
		--build-arg MEMCACHED_VERSION=$(MEMCACHED_VERSION) \
		--build-arg REDIS_VERSION=$(REDIS_VERSION) \
		--build-arg REDIS_NEXT_VERSION=$(REDIS_NEXT_VERSION) \
		--build-arg COMPOSER_VERSION=$(COMPOSER_VERSION) \
		--build-arg PRESTISSIMO_VERSION=$(PRESTISSIMO_VERSION) \
		--target nodejs-tideways \
		-t $(DOCKER_IMAGE_NAME):$(TAG)-nodejs-tideways .

test:
	docker run --rm $(DOCKER_IMAGE_NAME):$(TAG) php --version
	docker run --rm -u php \
		-e UPLOAD_MAX_SIZE=100M \
		-e MEMORY_LIMIT=1G \
		-e SYMFONY_ENV=prod \
		-e K8S_CONTEXT=1 \
		$(DOCKER_IMAGE_NAME):$(TAG) php -m
	docker run --rm -u php \
		-e MEMORY_LIMIT=1G \
		$(DOCKER_IMAGE_NAME):$(TAG) ls -lsah /usr/local/etc/php/conf.d/ | grep -q 'memory.ini'
	docker run --rm -u php \
		-e TIDEWAYS_API_KEY=abc123 \
		$(DOCKER_IMAGE_NAME):$(TAG)-tideways ls -lsah /usr/local/etc/php/conf.d/ | grep -q 'tideways.ini'
	docker run --rm $(DOCKER_IMAGE_NAME):$(TAG)-nodejs nodejs --version

tag:
	docker tag $(DOCKER_IMAGE_NAME):$(TAG)                 $(DOCKER_IMAGE_NAME):$(CUST_TAG)
	docker tag $(DOCKER_IMAGE_NAME):$(TAG)-tideways        $(DOCKER_IMAGE_NAME):$(CUST_TAG)-tideways
	docker tag $(DOCKER_IMAGE_NAME):$(TAG)-nodejs          $(DOCKER_IMAGE_NAME):$(CUST_TAG)-nodejs
	docker tag $(DOCKER_IMAGE_NAME):$(TAG)-nodejs-tideways $(DOCKER_IMAGE_NAME):$(CUST_TAG)-nodejs-tideways