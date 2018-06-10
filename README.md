# android-ci
# Continuous Integration using Jenkins to build Android applications

# 1) Invoke Gradle Wrapper (Plugin)
./ gradlew clean :app:generateStagingDebugSources :app:generateStagingDebugAndroidTestSources :app:mockableAndroidJar :app:assembleStagingDebug

# 2) Execute Shell to sent apk file to your intranet website

## Open project directory
cd /var/lib/jenkins/workspace/yourproject

## Last commit variable
export CHANGE_LOG="$(git log -1 --pretty=format:%s $GIT_COMMIT)"

## Defining new variables
export NEW_BRANCH=`echo $GIT_BRANCH | awk -Forigin/ '{print $2}'`<br />
export DATE_TIME=$(date +%d-%m-%Y--%T)

## Post method using curl
curl -H "Content-Type: application/json" -X POST -d '{"name":"'$BUILD_NUMBER'", "link":"http://ci.example.com:8080/job/yourproject/'$BUILD_NUMBER'/artifact/app/build/outputs/apk/staging/debug/app-staging-debug.apk", "branch":"'"$NEW_BRANCH"'", "production": false, "platform":"android", "changelog":"'"$CHANGE_LOG"'", "date":"'$DATE_TIME'"}' http://yourprojects.com.br/

# 3) Post-Build Actions
**/*debug.apk

brunomartins84[at]yahoo[dot]com[dot]br
