### json to dict

```python
json.loads(event['body'])
```

### dict to json

```python
json.dumps(Balances)
```

---

### **json dumps시 한글 파일 깨짐 해결**

```python
json.dumps(Results, ensure_ascii=False)
```