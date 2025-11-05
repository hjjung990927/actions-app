# **코리아 IT 아카데미 국비과정** 
## CI/CD & Load Balancing 😺
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

	➡️ [배포 워크플로우 파일 보기 (.github/workflows/ec2_aws_deploy.yml)](./.github/workflows/ec2_aws_deploy.yml)
	

> 모든 민감 정보는 `${{ secrets.변수명 }}` 형식으로 `.yml` 파일 내부에서 참조한다.  
> 코드에 직접 키를 작성하지 않아도 GitHub Actions 실행 시 자동으로 값이 주입된다.
---

#### CI/CD 워크플로우 단계 
<img width="1920" height="1080" alt="CI_CD" src="https://github.com/user-attachments/assets/f95425b9-dcf0-43b4-b5e1-5d1038444235" />

---

#### Load-Balancing(로드 밸런싱)

      서버나 시스템에 가해지는 네트워크 트래픽 과부화를 막기 위 여러 대의 서버에 분산시키는 기술이다.

#### Load-Balancing의 주요 기능 및 장점

   - 부하 분산: 클라이언트의 요청을 여러 대의 서버에 고르게 분배하여 한 서버에 집중되는 것을 방지합니다.
   - 고가용성: 특정 서버에 장애가 발생하더라도 서비스가 중단되지 않고, 다른 정상적인 서버로 요청을 보내 안정적인 서비스를 제공합니다.
   - 성능 확장: 서비스에 트래픽이 몰릴 때 서버를 추가하거나 제거하는 방식으로, 서비스의 중단 없이 유연하게 확장할 수 있습니다.
   - 장애 예방: 다운된 서버가 있을 경우 자동으로 감지하고 해당 서버로 요청을 보내지 않도록 관리하여 서비스의 안정성을 높입니다.

#### Load-Balancing의 주요 알고리즘

   - 라운드 로빈(Round Robin): 들어오는 요청을 여러 서버에 순차적으로 분배합니다.
   - 최소 연결(Least Connections): 현재 가장 적은 연결 수를 가진 서버로 요청을 보냅니다.
   - IP 해시(IP Hash): 클라이언트의 IP 주소를 기반으로 특정 서버에 요청을 고정시킵니다.

#### Nginx 설치
   ```
   ~$ sudo apt install -y nginx
   ~$ sudo systemctl status nginx
   ~$ sudo vim /etc/nginx/sites-available/(프로젝트 명)

      upstream (프로젝트 명) {
         least_conn; 	
         server ipaddress1:80;  # 첫 번째 EC2 	
         server ipaddress2:80;  # 두 번째 EC2
      }
      server {
         listen 80;

         location / {
            proxy_pass http://(프로젝트 명);
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
         }
      }
         
   ~$ sudo rm /etc/nginx/sites-enabled/default
   ~$ sudo ln -s /etc/nginx/sites-available/(프로젝트 명) /etc/nginx/sites-enabled/
   ~$ ls -l /etc/nginx/sites-enabled/
   ~$ sudo nginx -t
   ~$ sudo systemctl reload nginx~$ sudo systemctl status nginx
   ```

---

#### CI/CD → Load Balancing 연동 흐름
<img width="1920" height="1080" alt="Load-Balancing" src="https://github.com/user-attachments/assets/de134f76-03f3-47a4-aaeb-e4994bf87710" />

---

<p align="right">(<a href="#readme-top">back to top</a>)</p>
