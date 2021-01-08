# IntelliJ에서 Gradle 사용시 exploded war 배포

## 문제
IntelliJ, Gradle 프로젝트에서 톰캣으로 exploded war를 배포하는 중 문제 발생  
`Artifact Gradle : artifact.war (exploded): com.intellij.javaee.oss.admin.jmx.JmxAdminException: com.intellij.execution.ExecutionException: build-output-path/artifact.war not found for the web module.`

## 해결
- Gradle 프로젝트의 경우 빌드시 default로 intelliJ 빌드가 아닌 Gradle 빌드로 동작
- Gradle 플러그인은 빌드 output 경로로 exploded war를 생성하지 않는 문제가 있음 (archive war는 정상적으로 빌드됨)
  
### 해결방법1: IntelliJ 빌드 사용
Settings - Build, Execution, Deployment - Build Tools - Gradle - Build and run - default인 Gradle 대신 intelliJ 선택

### 해결방법2: gradle task 추가
1. build.gradle에 task 추가
 ```groovy
 task explodedWar(type: Copy) {
   into "${buildDir}/libs/exploded-war"
   with war
 }  
 ```
2. Tomcat Edit Configurations - Deployment - Deploy at the server startup - External Source로 빌드된 exploded war 추가
3. Tomcat Edit Configurations - Before launch - Run Gradle Task 추가 - task: explodedWar