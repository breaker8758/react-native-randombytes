# react-native-randombytes

react-native-randombytes 라이브러리의 이슈가 있는데, 수정하려는 의지가 보이지 않아 문제 되는 부분을 에러만 발생하지 않아 수정 후 재배포 합니다.
- 원본 라이브러리 : https://github.com/mvayngrib/react-native-randombytes
- 라이브러리 이슈 : https://github.com/mvayngrib/react-native-randombytes/issues/40

## 수정한 부분

```js
function init () {
  // RNRandomBytes && 추가
  if (RNRandomBytes && RNRandomBytes.seed) {
    let seedBuffer = toBuffer(RNRandomBytes.seed)
    addEntropy(seedBuffer)
  } else {
    seedSJCL()
  }
}

export function randomBytes (length, cb) {
  if (!cb) {
    let size = length
    let wordCount = Math.ceil(size * 0.25)
    let randomBytes = sjcl.random.randomWords(wordCount, 10)
    let hexString = sjcl.codec.hex.fromBits(randomBytes)
    hexString = hexString.substr(0, size * 2)
    return new Buffer(hexString, 'hex')
  }

  // RNRandomBytes && 추가
  RNRandomBytes && RNRandomBytes.randomBytes(length, function(err, base64String) {
    if (err) {
      cb(err)
    } else {
      cb(null, toBuffer(base64String))
    }
  })
}
```

## 설치 방법

```bash
npm install https://github.com/breaker8758/react-native-randombytes
```
