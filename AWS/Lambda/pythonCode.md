### **s3에서 이미지 가져오기**

```python
#set Image
    S3URL = boto3.client('s3').generate_presigned_url(ClientMethod='get_object', Params={'Bucket': 'mepark-brphotos', 'Key': PostedBodys['PartnerBN']})
    PartnerResult['Item']['ImgUrl'] = S3URL
```

---
### **검색 메커니즘**

```python
#set getParked Cars
    if PostedBodys['Mode'] == 'DEFAULT' : #모드 1 : DEFAULT
    
        ParkedCarsResult = resource('dynamodb').Table('mepark-RecordParkingLots').query(KeyConditionExpression=Key('ParkingLN').eq(PostedBodys['ParkingLN']))['Items']
    
    elif PostedBodys['Mode'] == 'SEARCH' : #모드 2 : 차량 번호 4자리 혹은 연락처 4자리 검색
        
        ParkedCarsResult = resource('dynamodb').Table('mepark-RecordParkingLots').scan(FilterExpression = Attr('Contact').contains(PostedBodys['Keyword']) | Attr('LP').contains(PostedBodys['Keyword']))['Items']
    
    elif PostedBodys['Mode'] == 'DATE' : #모드 3 : 날짜 검색
    
        ParkedCarsResult = resource('dynamodb').Table('mepark-RecordParkingLots').query(IndexName='EnterDate-ParkingLN-index', KeyConditionExpression=Key('EnterDate').eq(PostedBodys['Date']) & Key('ParkingLN').eq(PostedBodys['ParkingLN']))['Items']
```