GROUP = com.almeros.android-gesture-detectors
ARTIFACT = library

TGT_DIR = ./build/outputs/aar
AAR_FILE = $(TGT_DIR)/library-release.aar
MAVEN_SERVER = 
MAVEN_SSH_PORT = 
MAVEN_PATH = 
MAVEN_URL = scpexe://$(MAVEN_SERVER):$(MAVEN_SSH_PORT)/$(MAVEN_PATH)
LOCAL_DIR = /tmp/mavenrepo

.PHONY: build deploy local clean

build:
	../gradlew build

deploy: build
	@if [ -z "$(VERSION)" ]; then \
		echo "Usage:\n  make deploy VERSION=x.y.z\n"; \
		exit 1; \
	fi
	mvn deploy:deploy-file -DgroupId=$(GROUP) -DartifactId=$(ARTIFACT) \
		-Dversion=$(VERSION) \
		-Dpackaging=aar -Dfile=$(AAR_FILE) \
		-Durl=$(MAVEN_URL)
	ssh -t $(MAVEN_SERVER) -p $(MAVEN_SSH_PORT) \
		'd=/$(MAVEN_PATH)/$(GROUP) && ' \
		'w=$$(stat -c "%A" $$d | cut -c6) && [ $$w = "w" ] || chmod g+w -R $$d'

local: build
	mkdir -p $(LOCAL_DIR)
	mvn deploy:deploy-file -DgroupId=$(GROUP) -DartifactId=$(ARTIFACT) \
		-Dversion=dev \
		-Dpackaging=aar -Dfile=$(AAR_FILE) \
		-Durl=file://$(LOCAL_DIR)

clean:
	../gradlew clean
	rm -rf $(TGT_DIR)
	rm -rf $(LOCAL_DIR)
