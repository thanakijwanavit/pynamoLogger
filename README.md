# PynamoLogger
> log information into a dynamodb, with dax support


## Install

`pip install pynamologger`<br>
# view [Documentation](https://thanakijwanavit.github.io/pynamoLogger/)

# Usage

## create a dynamodb table 
#### indexKey
* appName

#### rangeKey
* timestamp

### Sam template
put this in your template.yaml file under properties section<br>
make sure that the table name is the same in Meta declaration
```yaml
YourLogTable:
  Type: AWS::DynamoDB::Table
  Properties:
    AttributeDefinitions:
      - AttributeName: appName
        AttributeType: S
      - AttributeName: timestamp
        AttributeType: N
    BillingMode: PAY_PER_REQUEST
    TableName: <put your table name here>
    KeySchema:
      - AttributeName: appName
        KeyType: HASH
      - AttributeName: timestamp
        KeyType: RANGE

```
### create using pynamodb api
```python
Logger.create_table()
```

## class meta definition

```python
from pynamoLogger.logger import PynamoLogger
class Logger(PynamoLogger):
  class Meta:
    table_name = 'member-database-log-dev-manual' # put your table name
    region = 'ap-southeast-1' #put your region name
    billing_mode = 'PAY_PER_REQUEST'
    
```

```python
# Logger.create_table()
```

```python
Logger.log(appName = 'test', message = "this is crazy", responseObject = {'response': 'null response'})
```




    {'logId': 1603075789.054751,
     'saveResult': {'ConsumedCapacity': {'CapacityUnits': 1.0,
       'TableName': 'member-database-log-dev-manual'}}}



```python
Logger.checkLog(appName = 'test', logId = '1603074235.103042')
```




    [{'appName': None,
      'timestamp': 1603074235.103042,
      'logMessage': 'this is crazy',
      'requestObject': {'noValue': 'noValue'},
      'responseObject': {'response': 'null response'}}]


