# Config .DS for ILM

## Create ILM
1. Stack Management > Index Lifecycle Policies > Create policy

## View data streams

1. Stack Management > Index Management > Data Streams > Component Template  
2. Index template   
3. Manage > Edit > Index settings  
```
{
  "lifecycle": {
    "name": "Your Policy"
  }
}
```
4. Save template
5. Roll over 
```
POST /your_template/_rollover/
```