/**
* NOTE: THIS JENKINSFILE IS GENERATED VIA "./hack/run update"
*
* DO NOT EDIT IT DIRECTLY.
*/
node {
        def variants = "default,alpine".split(',');
        for (int v = 0; v < variants.length; v++) {

                def versions = "1.7,1.8".split(',');
                for (int i = 0; i < versions.length; i++) {

                  if (variants[v] == "default") {
                    variant = ""
                    tag = "${versions[i]}"
                  } else {
                    variant = variants[v]
                    tag = "${versions[i]}-${variant}"
                  }


                        try {
                                stage("Build (Golang-${tag})") {
                                        openshift.withCluster() {
        openshift.apply([
                                "apiVersion" : "v1",
                                "items" : [
                                        [
                                                "apiVersion" : "v1",
                                                "kind" : "ImageStream",
                                                "metadata" : [
                                                        "name" : "golang",
                                                        "labels" : [
                                                                "builder" : "s2i-golang"
                                                        ]
                                                ],
                                                "spec" : [
                                                        "tags" : [
                                                                [
                                                                        "name" : "${tag}",
                                                                        "from" : [
                                                                                "kind" : "DockerImage",
                                                                                "name" : "golang:${versions[i]}",
                                                                        ],
                                                                        "referencePolicy" : [
                                                                                "type" : "Source"
                                                                        ]
                                                                ]
                                                        ]
                                                ]
                                        ],
                                        [
                                                "apiVersion" : "v1",
                                                "kind" : "ImageStream",
                                                "metadata" : [
                                                        "name" : "s2i-golang",
                                                        "labels" : [
                                                                "builder" : "s2i-golang"
                                                        ]
                                                ]
                                        ]
                                ],
                                "kind" : "List"
                        ])
        openshift.apply([
                                "apiVersion" : "v1",
                                "kind" : "BuildConfig",
                                "metadata" : [
                                        "name" : "s2i-golang-${tag}",
                                        "labels" : [
                                                "builder" : "s2i-golang"
                                        ]
                                ],
                                "spec" : [
                                        "output" : [
                                                "to" : [
                                                        "kind" : "ImageStreamTag",
                                                        "name" : "s2i-golang:${tag}"
                                                ]
                                        ],
                                        "runPolicy" : "Serial",
                                        "resources" : [
                                            "limits" : [
                                                "memory" : "2.5Gi"
                                            ]
                                        ],
                                        "source" : [
                                                "git" : [
                                                        "uri" : "https://github.com/ausnimbus/s2i-golang"
                                                ],
                                                "type" : "Git"
                                        ],
                                        "strategy" : [
                                                "dockerStrategy" : [
                                                        "dockerfilePath" : "versions/${versions[i]}/${variant}/Dockerfile",
                                                        "from" : [
                                                                "kind" : "ImageStreamTag",
                                                                "name" : "golang:${tag}"
                                                        ]
                                                ],
                                                "type" : "Docker"
                                        ]
                                ]
                        ])
        echo "Created s2i-golang:${tag} objects"
        /**
        * TODO: Replace the sleep with import-image
        * openshift.importImage("golang:${tag}")
        */
        sleep 60

        echo "==============================="
        echo "Starting build s2i-golang-${tag}"
        echo "==============================="
        def builds = openshift.startBuild("s2i-golang-${tag}");

        timeout(60) {
                builds.untilEach(1) {
                        return it.object().status.phase == "Complete"
                }
        }
        echo "Finished build ${builds.names()}"
        builds.logs()
}

                                }
                                stage("Test (Golang-${tag})") {
                                        openshift.withCluster() {
        echo "==============================="
        echo "Starting test application"
        echo "==============================="

        def testApp = openshift.newApp("https://github.com/ausnimbus/golang-ex", "--image-stream=s2i-golang:${tag}", "-l app=golang-ex");
        echo "new-app created ${testApp.count()} objects named: ${testApp.names()}"
        testApp.describe()

        def testAppBC = testApp.narrow("bc");
        def testAppBuilds = testAppBC.related("builds");
        echo "Waiting for ${testAppBuilds.names()} to finish"
        timeout(10) {
                testAppBuilds.untilEach(1) {
                        return it.object().status.phase == "Complete"
                }
        }
        echo "Finished ${testAppBuilds.names()}"

        def testAppDC = testApp.narrow("dc");
        echo "Waiting for ${testAppDC.names()} to start"
        timeout(10) {
                testAppDC.untilEach(1) {
                        return it.object().status.availableReplicas >= 1
                }
        }
        echo "${testAppDC.names()} is ready"

        def testAppService = testApp.narrow("svc");
        def testAppHost = testAppService.object().spec.clusterIP;
        def testAppPort = testAppService.object().spec.ports[0].port;

        sleep 60
        echo "Testing endpoint ${testAppHost}:${testAppPort}"
        sh "curl -o /dev/null $testAppHost:$testAppPort"
}

                                }
                                stage("Stage (Golang-${tag})") {
                                        openshift.withCluster() {
        echo "==============================="
        echo "Tag new image into staging"
        echo "==============================="

        openshift.tag("ausnimbus-ci/s2i-golang:${tag}", "ausnimbus/s2i-golang:${tag}")
}

                                }
                        } finally {
                                openshift.withCluster() {
                                        echo "Deleting test resources golang-ex"
                                        openshift.selector("dc", [app: "golang-ex"]).delete()
                                        openshift.selector("bc", [app: "golang-ex"]).delete()
                                        openshift.selector("svc", [app: "golang-ex"]).delete()
                                        openshift.selector("is", [app: "golang-ex"]).delete()
                                        openshift.selector("pods", [app: "golang-ex"]).delete()
                                        openshift.selector("routes", [app: "golang-ex"]).delete()
                                }
                        }

                }
        }
}
