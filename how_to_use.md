

# How to read provided file in FastAPI:
```py
@app.post("/api/categorize")
async def read_root(columnName: str = Form(), csvFile: UploadFile = File(...)):
    content: bytes = await csvFile.read() # raw bytes of provided file
    ...
```



# How to make requests using `requests`:
```py
from pathlib import Path
import requests
request_file = Path(...)

fileobj = request_file.open('rb')
server_url = 'http://localhost:6000'
response = requests.post(
    url=f'{server_url}/api/categorize',
    data={'columnName': "foobar"},
    files={'file': ("data.csv", fileobj, 'multipart/form-data')},
)

response_file = Path(...)
if response.status_code == 200:
    # now response.content holds raw bytes of response file
    response_file.write_bytes(response.content)
    print("File downloaded successfully!")
else:
    print(f"Error: {response.status_code}")
```
