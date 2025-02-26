# github workflow action test app

## INFO

GitHub Action을 사용하여  
release 브랜치에 push 이벤트가 감지되면  
apk를 build하고  
build된 결과를  
rclone을 사용하여  
google drive에 업로드하는 어플

## GOOGLE CONSOLE

- project 생성
- service account 생성
    - role: Editor
    - key: json

## RCLONE

- rclone config
    - 경로: ~/.config/rclone/rclone.conf
    - 내용<a id="rclone-config"/>
      ```
      - type: drive
      - scope: drive
      - service_account_file: sa.json
      - team_drive:
      ```

## GITHUB SECRETS

- github project -> Settings -> Secrets -> Actions -> New repository secret 항목 추가
- service account key
    - name: GDRIVE_SERVICE_ACCOUNT_JSON
    - value: service account key(json)
- rclone config
    - name: GDRIVE_RCLONE_CONFIG
    - value: rclone config

## 동작

1. git checkout
2. setup jdk
3. set gradlew permission
4. build release
5. vm에 rclone 설치
6. github secrets service account key vm에 저장
7. github secrets rclone config vm에 저장
8. build된 apk를 rclone copy를 사용하여 google drive에 업로드

## TODO

- google console project 설정
- rclone config
- service account key(json)
- drive path
- config, service account key를 base64로 저장, yml에서 decode하여 사용
- 현재 서비스 자체 드라이브에 업로드함. 사용자 계정에 공유되어 그곳에 업데이트 해야함
- gradlew 로 앱 빌드 시 keystore 필요
    - system app 을 위함
    - vm에서 keystore 로드하는 방법에 대해 필요
    - 아니면 secrets에 base64로 인코딩하여 넣을 수 있도록
    - 릴리즈 시 파일 명에 버전 네임 필수
- **Firebase App Distribution Github Action 사용 고려**(https://github.com/marketplace/actions/firebase-app-distribution)
```
  name: Upload artifact to Firebase Distribution with testers
  uses: ./
  with:
    appId: ${{secrets.FIREBASE_APP_ID}}
    token: ${{secrets.FIREBASE_TOKEN}}
    testers: "test@test.com, test2@test2.com"
    releaseNotesFile: README.md
    file: ApiDemos-debug.apk
```
