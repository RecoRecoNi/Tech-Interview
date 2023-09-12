# CI / CD

## 💡 CI/CD가 무엇인지 설명해주시고 간단한 사용 사례를 들어주세요.

CI/CD는 애플리케이션 개발 단계를 자동화하여 애플리케이션을 더욱 짧은 주기로 고객에게 제공하는 방법입니다.

**`Continuous Integration, 지속적 통합`**

→ 빌드, 테스트 자동화

- 새롭게 작성한 코드 변경 사항이 Build, Test 진행한 후 Test Case에 통과했는지 확인
- 지속적으로 코드 품질 관리

**`Continuous Deploy/Delivery, 지속적 배포`**

→ 배포 자동화

- **Continuous Delivery** : 공유 레포지토리로 자동으로 Release하는 것으로, Production까지의 자동화는 이루어지지 않습니다.
- **Continuous Deploy** : Production 레벨까지 자동으로 deploy하는 것을 의미합니다.

1. 작성한 코드가 항상 신뢰 가능한 상태가 되면 자동으로 배포될 수 있도록 하는 과정
2. CI 이후 CD를 진행
3. dev / staging / main 브랜치에 Merge가 될 경우 코드가 자동으로 서버에 배포

**`사용 사례`**

데이터 플랫폼을 개발할 때 `React with Typescript`를 사용하여 하나의 새로운 기능을 추가하였습니다.

해당 기능을 PR로 올렸을 때 깃허브 액션은 해당 코드를 가지고 컨테이너를 가동시켜 주어진 테스트를 통과하는지 확인하였으며 테스트가 다 통과되고 나니 Approve를 할 수 있게 해주었으며 자동으로 연결해주었습니다.

이후 Approve가 되어 Git에 Repo에 코드가 완벽히 등록되었을 때 깃허브 액션은 도커 이미지를 생성하여 사내 Harbor에 이미지를 Deploy 시켰고 사내 K8S에서 새로운 이미지를 다시 배포하는 과정을 자동으로 이뤄내는 과정을 볼 수 있었습니다.

<br>

## 📚 Reference

[RedHat - CI/CD(Continuous Integration / Continuous Delivery)란?](https://www.redhat.com/ko/topics/devops/what-is-ci-cd)  
[티스토리 - CI/CD란 무엇인가 (Feat. DevOps 엔지니어)](https://artist-developer.tistory.com/24)
