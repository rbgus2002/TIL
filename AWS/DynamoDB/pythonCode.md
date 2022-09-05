### Module

```python
import boto3
from boto3.dynamodb.conditions import Key, Attr
```

---

### Query

- 일반 query

```python
CarResults = boto3.resource('dynamodb').Table('mepark-RecordParkingLots').query(KeyConditionExpression=Key('ParkingLN').eq(PostedBodys['ParkingLN']))['Items']
ParkingLotResults = boto3.resource('dynamodb').Table('mepark-ParkingLots').query(KeyConditionExpression = Key('PartnerBN').eq(UserInfo['Item']['PartnerBN']))['Items']
VisitPlaces = boto3.resource('dynamodb').Table('mepark-VisitPlaces').query(KeyConditionExpression=Key('ParkingLN').eq(ThisItem['ParkingLN']))['Items']
```

- 내림차순 정렬 && 날짜 필터 query

```python
#set Recode
RecodeResults = boto3.resource('dynamodb').Table('mepark-RecordParkingLots').query(
KeyConditionExpression=Key('ParkingLN').eq(ThisItem['ParkingLN']), 
FilterExpression=Attr('EnterDate').gte(PostedBodys['StartDate']) & Attr('EnterDate').lt(PostedBodys['EndDate']) & Attr('is_use').eq('Y'),
ScanIndexForward=False
)
```

- GSI query

```python
# 최근 출근 기록 가져오기
DDBResult = boto3.resource('dynamodb').Table('mepark-AttendanceManagement').query (
    IndexName='UserID-AMID-index',
    KeyConditionExpression=Key('UserID').eq(UserID),
    ScanIndexForward=False,
    Limit=1 
)
```

```python
boto3.resource('dynamodb').Table('mepark-Partners').query(
				IndexName='Status-PartnerBN-index', 
				KeyConditionExpression=Key('Status').eq('Pending')
)['Items']
```



- PK + 특정 Attribution로 query

```python
Attendances = boto3.resource('dynamodb').Table('mepark-AttendanceManagement').query(
        KeyConditionExpression=Key('ParkingLN').eq(PostedBodys['ParkingLN']),
        FilterExpression = 'UserID = :v',
        ExpressionAttributeValues= {
        	':v': PostedBodys['UserID'],
        })['Items']
```

---

### get_item

```python
ParkingLNResult = boto3.resource('dynamodb').Table('mepark-ParkingLots').get_item(Key = {'PartnerBN':UserInfo['Item']['PartnerBN'], 'ParkingLN':PostedBodys['ParkingLN']})
```

---

### scan

- filter eq

```python
ParkingLotResults = resource('dynamodb').Table('mepark-ParkingLots').scan(FilterExpression=Key('PartnerBN').eq(UserInfo['Item']['PartnerBN']))['Items']
```

- filter begins_with

```python
PartnerResults = resource('dynamodb').Table('mepark-Partners').scan(FilterExpression = Attr('PartnerName').begins_with(PostedBodys['Keyword']))['Items']
```

- contains

```python
ParkedCarsResult = resource('dynamodb').Table('mepark-RecordParkingLots').scan(FilterExpression = Attr('Contact').contains(PostedBodys['Keyword']) | Attr('LP').contains(PostedBodys['Keyword']))['Items']
```

---

### put_item

```python
boto3.resource('dynamodb').Table('Accounts').put_item(Item = NewAccount)
```

---
