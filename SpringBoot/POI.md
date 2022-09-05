## Excel to DB

### 1. build.gradle에 의존성 추가

```java
//POI
implementation group: 'org.apache.poi', name: 'poi', version: '5.0.0'
implementation group: 'org.apache.poi', name: 'poi-ooxml', version: '5.0.0'
```

### 2. POI 라이브러리 활용 (엑셀 파일 읽기)
```java
public void readExcelAndSave() throws IOException {
        String fileLocation = "엑셀 파일 경로";
        FileInputStream file = new FileInputStream(fileLocation);
        Workbook workbook = new XSSFWorkbook(file);
        Sheet sheet = workbook.getSheetAt(0);

        // Sheet로 읽어들인 데이터를 다시 Row와 Cell로 나눈다.
				List<PublicTrashAddress> list = new ArrayList<>();
        for (int j = 2; j < 5376; j++) {
            Row row = sheet.getRow(j);

            // set address
            String address = "서울 ";
            address += row.getCell(2).toString() + " ";
            address += row.getCell(3).toString() + " ";
            address += row.getCell(4).toString();

            //set kind
            String kind = divideKind(row.getCell(6).toString());

            //set spec
            String spec = row.getCell(5).toString().strip();

            PublicTrashAddress publicTrashAddress = PublicTrashAddress.builder()
                    .address(address)
                    .kind(kind)
                    .spec(spec)
                    .build();

            // save
            publicTrashAddressRepository.save(publicTrashAddress);
}
```