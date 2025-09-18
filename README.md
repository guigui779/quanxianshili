workflows:
  ios-build-demo:
    name: iOS Build OpenIM Demo
    max_build_duration: 60
    environment:
      vars:
        REPO_URL: "https://github.com/openimsdk/openim-ios-demo.git"
        BRANCH: "main"
      cocoapods: true
      xcode: latest
    scripts:
      - name: Clone repository
        script: |
          if [ ! -d "openim-ios-demo" ]; then
            git clone $REPO_URL
          else
            cd openim-ios-demo
            git checkout $BRANCH
            git pull origin $BRANCH
            cd ..
          fi

      - name: Install Pods
        script: |
          cd openim-ios-demo/Example
          pod install --repo-update

      - name: Build iOS
        script: |
          cd openim-ios-demo/Example
          xcodebuild \
            -workspace OpenIMSDKUIKit.xcworkspace \
            -scheme OpenIMSDKUIKit \
            -sdk iphoneos \
            -configuration Release \
            CODE_SIGNING_ALLOWED=NO
