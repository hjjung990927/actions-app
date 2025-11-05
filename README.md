# **코리아 IT 아카데미 국비과정** 
## CI/CD 😺
<a name="readme-top"></a> 

#### Docker(컨테이너 기반 가상화 기술)

    Docker는 애플리케이션을 컨테이너라는 표준화된 단위로 패키징하여, 
    개발부터 테스트, 배포까지 전 과정에서 일관성 있고 안정적인 실행을 가능하게 하는 컨테이너 기술이다.

#### Docker의 주요 개념

  - **Image**: 실행 가능한 환경과 코드를 담은 템플릿(=컨테이너의 설계도)
  - **Container**: 이미지를 실제로 실행한 인스턴스(=실행 중인 애플리케이션)
  - **Dockerfile**: 이미지를 자동으로 생성하기 위한 설정 파일

#### CI/CD(지속적 통합 / 지속적 배포)

  - CI(Continuous Integration): 코드 변경 사항을 주기적으로 통합하고 자동으로 빌드 및 테스트하는 과정
  - CD(Continuous Deployment): 통합된 애플리케이션을 자동으로 서버에 배포하는 과정

        CI/CD는 개발자가 코드를 Push하면 자동으로
    	빌드(Build) → 테스트(Test) → 배포(Deploy)가 진행되도록 해준다.

#### GitHub Secrets 설정
  - 코드 내에 서버 정보나 비밀키를 직접 노출하지 않기 위해 GitHub의 `Settings → Secrets and variables → Actions` 에 변수를 등록해준다.

  - 예시 구조

    | 이름 | 설명 | 예시 |
    |------|------|------|
    | **EC2_HOST** | 배포 대상 EC2의 IP | `9.27.xxx.xxx` |
    | **EC2_USER** | EC2 계정명 | `ubuntu` |
    | **EC2_KEY** | EC2 SSH 개인키(`.pem` 파일 전체 복사) | `-----BEGIN OPENSSH PRIVATE KEY----- ...` |

#### GitHub Actions(워크플로우 자동화 도구)
  - GitHub에서 제공하는 CI/CD 서비스로, 저장소 내 `.github/workflows/` 폴더에 YML 파일을 작성하여 배포 파이프라인을 구성한다.

#### Workflow 파일 및 Dockerfile 파일

| 파일 | 설명 | 링크 |
|------|--------|--------|
|`ec2_aws_deploy.yml`| GitHub Actions 배포 설정| 🔗 [보기](https://github.com/hjjung990927/actions-app/blob/master/.github/workflows/ec2_aws_deploy.yml)|
|`Dockerfile`| Docker 빌드 설정| 📦 [보기](https://github.com/hjjung990927/actions-app/blob/master/Dockerfile)|
	

> 모든 민감 정보는 `${{ secrets.변수명 }}` 형식으로 `.yml` 파일 내부에서 참조한다.  
> 코드에 직접 키를 작성하지 않아도 GitHub Actions 실행 시 자동으로 값이 주입된다.
---

#### CI/CD 워크플로우 단계 
<img width="1920" height="1080" alt="CI_CD" src="https://github.com/user-attachments/assets/f95425b9-dcf0-43b4-b5e1-5d1038444235" />

---

<p align="right">(<a href="#readme-top">back to top</a>)</p>
