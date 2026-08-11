# card-extractor — 배포용 저장소

**PDF 거래내역 추출기**의 릴리스 산출물만 두는 저장소입니다. 소스 코드는 별도 비공개
저장소에 있습니다.

## 왜 저장소를 나눴나

앱은 `electron-updater`로 이 저장소의 **GitHub Releases**를 확인해 업데이트를 받습니다.
GitHub Releases는 저장소가 비공개면 다운로드에 토큰이 필요한데, 그 토큰을 앱에 넣으면
사용자가 꺼내 볼 수 있어 공개한 것과 같아집니다. 그래서 **코드는 비공개, 산출물만 공개**로
분리했습니다.

## 릴리스에 포함되는 파일

| 파일 | 용도 |
|---|---|
| `card-extractor-setup-<버전>.exe` | NSIS 인스톨러 (자동 업데이트 대상) |
| `card-extractor-setup-<버전>.exe.blockmap` | 차등 다운로드용 블록맵 |
| `card-extractor-<버전>.exe` | 포터블 (설치 없이 실행) |
| `latest.yml` | 업데이터가 읽는 버전 메타 (sha512·size 포함) |

`latest.yml`의 해시·크기가 실제 exe와 다르면 업데이트가 검증에서 실패하므로,
**세 파일은 항상 같은 빌드에서 나온 것을 함께** 올려야 합니다.

## 배포 방법

소스 저장소에서:

```bash
cd electron
GH_TOKEN=<repo Contents 쓰기 권한 토큰> npm run build:win -- --publish always
```

electron-builder가 draft 릴리스를 만들고 자산명을 ASCII로 정규화해 올립니다.
확인 후 GitHub에서 **Publish release**를 눌러 공개합니다.
