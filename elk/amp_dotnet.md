# Windows
## [Dowload](https://github.com/elastic/apm-agent-dotnet/releases) profile.dll  


## Config file webconfig for .NetFramework
```
<configuration>
  <appSettings>
    <add key="ElasticApm:ServerUrl" value="http://ip-apm-server:8200" />
    <add key="ElasticApm:ServiceName" value="service-name-of-apm" />
    <add key="ElasticApm:FullFrameworkConfigurationReaderType" value="Name-of-App(WebApplication1)" />
  </appSettings>
</configuration>
```

## Set  variable Global for before IIS 10  
```
$profilerHomeDir = ""<unzipped directory>" "
$environment = @{
  COR_ENABLE_PROFILING = "0"
  COR_PROFILER = "{FA65FE15-F085-4681-9B20-95E04F6C03CC}"
  COR_PROFILER_PATH = "$profilerHomeDir\elastic_apm_profiler.dll"
  ELASTIC_APM_PROFILER_HOME = "$profilerHomeDir"
  ELASTIC_APM_PROFILER_INTEGRATIONS = "$profilerHomeDir\integrations.yml"
  COMPlus_LoaderOptimization = "1" 
}
$environment.Keys | ForEach-Object {
  [Environment]::SetEnvironmentVariable($_, $environment[$_], "Machine")
}
```
## Set variable for app pool for IIS 10
```
$appcmd = "$($env:systemroot)\system32\inetsrv\AppCmd.exe"
$appPool = "<application-pool>" 
$profilerHomeDir = "<unzipped directory>" 
$environment = @{
  COR_ENABLE_PROFILING = "1"
  COR_PROFILER = "{FA65FE15-F085-4681-9B20-95E04F6C03CC}"
  COR_PROFILER_PATH = "$profilerHomeDir\elastic_apm_profiler.dll"
  ELASTIC_APM_PROFILER_HOME = "$profilerHomeDir"
  ELASTIC_APM_PROFILER_INTEGRATIONS = "$profilerHomeDir\integrations.yml"
  COMPlus_LoaderOptimization = "1" 
}

$environment.Keys | ForEach-Object {
  & $appcmd set config -section:system.applicationHost/applicationPools /+"[name='$appPool'].environmentVariables.[name='$_',value='$($environment[$_])']"
}
```