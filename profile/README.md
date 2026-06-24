# ACE_VR

> **초등학생 대상 Meta SDK 핸드트래킹 기반 VR 페인팅 프로젝트**  
> 컨트롤러 없이 손동작으로 도구를 잡고, 색을 선택하고, 다양한 페인팅 도구로 3D 오브젝트를 색칠하는 VR 콘텐츠입니다.

<br/>

<p align="center">
  <img width="500" alt="페인팅 오브젝트 및 도구 선택 UI" src="https://github.com/user-attachments/assets/a4d417f3-a9c2-49b1-8ad3-da52c5d78a31" />
</p>
[시연영상 보러가기](https://drive.google.com/file/d/1KOBWu-WBkeJ4npNvVBOGWZyHcTP1mY-f/view?usp=drive_link)

---

## 프로젝트 소개

**ACE_VR**은 초등학생을 대상으로 한 VR 사용성 연구를 위해 제작한 **Meta SDK 핸드트래킹 기반 VR 페인팅 콘텐츠**입니다.  
컨트롤러 조작에 익숙하지 않은 사용자도 손동작만으로 자연스럽게 VR 콘텐츠를 체험할 수 있도록 설계했습니다.

이 프로젝트는 단순히 VR 공간에서 그림을 감상하는 콘텐츠가 아니라, 사용자가 직접 오브젝트를 선택하고 색을 입히며 자신만의 작품을 완성하는 **참여형 VR 미술 경험**을 목표로 했습니다.  
특히 초등학생 사용자가 복잡한 조작 없이도 미술 활동처럼 받아들일 수 있는 직관적인 체험을 제공하는 데 중점을 두었습니다.

ACE_VR은 VR 기술을 교육·전시·창작 활동과 연결해, 사용자가 가상 공간 안에서 능동적으로 표현하고 결과물을 남길 수 있는 경험을 제안한 프로젝트입니다.

---

## 프로젝트 정보

| 항목 | 내용 |
|---|---|
| 개발 기간 | 2024.12 ~ 2025.05 |
| 대상 플랫폼 | Meta Quest / VR |
| 기술 스택 | Unity, C#, Meta SDK |
| 주요 기능 | 핸드트래킹 인터랙션, 실시간 텍스처 페인팅, 결과물 저장 및 서버 전송 |

---

## 주요 기능

### 1. Meta SDK 핸드트래킹 인터랙션

Meta SDK의 **Grab / Poke 인터랙션**을 활용하여 사용자가 실제 도구를 손으로 잡고 사용하는 것처럼 조작할 수 있도록 구현했습니다.  
기본 Grab 기능만으로는 “도구를 잡은 뒤 특정 손동작을 취했을 때 기능이 실행되는 도구”를 구현하기 어려워, `IHandGrabUseDelegate`를 상속해 Grab 이후의 Use 동작을 커스텀 제어했습니다.

- 커스텀 핸드 포즈 2종 사용: **잡기 모션 / 누르기 모션**
- 입력 강도 `0.9` 이상일 때 스프레이 분사
- 입력 강도 `0.3` 이하일 때 재입력 가능 상태로 전환
- 좌·우 손 Grab 상태 및 트리거 이벤트 로그 기록

---

### 2. 실시간 UV 기반 페인팅 시스템

초기에는 파티클의 Collision 이벤트를 활용해 충돌 지점을 감지하는 방식으로 접근했지만, Collision만으로는 모델 텍스처의 어느 위치에 색상을 적용해야 하는지 정확한 좌표를 얻기 어려웠습니다.

이를 해결하기 위해 파티클이 발사되는 방향으로 Raycast를 수행하고, 충돌 지점에서 반환되는 `RaycastHit.textureCoord`를 활용하여 해당 UV 좌표에 색상을 적용하는 방식으로 구현했습니다.

이를 통해 단순 파티클 이펙트가 아니라, 실제 오브젝트 표면에 색상이 누적되는 페인팅 결과물을 만들 수 있었습니다.

---

### 3. 다양한 페인팅 도구와 페인팅 오브젝트


사용자는 VR 공간에서 색칠하고 싶은 **페인팅 오브젝트**를 먼저 선택한 뒤, 손가락·스프레이·물총·물감폭탄 등 다양한 도구를 활용해 오브젝트를 직접 페인팅할 수 있습니다.

이를 통해 단순히 정해진 대상을 색칠하는 것이 아니라, 사용자가 원하는 작품을 선택하고 그에 맞는 도구와 색상을 조합해 표현하는 능동적인 VR 페인팅 경험을 제공했습니다.

<p align="center">
  <img width="600" alt="도구 선택 UI" src="https://github.com/user-attachments/assets/5b5c7b06-189e-427f-9af0-5db401fb04cf" />
</p>

<br/>

각 도구는 서로 다른 조작 방식과 피드백을 제공하도록 구성했습니다.  
도구 사용 시 분사 이펙트와 사운드 피드백이 함께 재생되며, 대상 오브젝트의 텍스처에 색상이 실시간으로 반영되도록 구현했습니다.

이를 통해 사용자는 원하는 오브젝트를 선택하고, 도구와 색상을 조합해 창의적으로 작품을 완성할 수 있습니다.

<table>
  <tr>
    <td align="center" width="25%">
      <b>페인트 폭탄</b><br/>
      <sub>넓은 범위에 색상을 입히는 범위형 도구</sub>
    </td>
    <td align="center" width="25%">
      <b>페인트 건</b><br/>
      <sub>발사형 입력을 통해 색상 분사</sub>
    </td>
    <td align="center" width="25%">
      <b>페인트 스프레이</b><br/>
      <sub>잡고 누르는 포즈로 파티클 분사</sub>
    </td>
    <td align="center" width="25%">
      <b>페인트 손가락</b><br/>
      <sub>손가락으로 직접 표면 색칠</sub>
    </td>
  </tr>
  <tr>
    <td>
      <img width="100%" alt="물감폭탄" src="https://github.com/user-attachments/assets/b63c1835-5d1f-4d38-89b4-b467dea8f1bd" />
    </td>
    <td>
      <img width="100%" alt="페인트 건" src="https://github.com/user-attachments/assets/e8362801-2df8-4dcc-a58e-c9a0ac1c71a3" />
    </td>
    <td>
      <img width="100%" alt="페인트 스프레이" src="https://github.com/user-attachments/assets/c94f9236-02ea-43a4-a6f7-b2dbea70151f" />
    </td>
    <td>
      <img width="100%" alt="페인트 손가락" src="https://github.com/user-attachments/assets/f992eb49-9653-48f7-b68d-8d2d9ba9e864" />
    </td>
  </tr>
</table>

<br/>

---

### 4. 페인팅 초기화 기능

페인팅된 오브젝트를 초기 상태로 되돌리는 초기화 기능을 구현했습니다.  
사용자는 색칠 결과가 마음에 들지 않거나 다른 색상 조합을 시도하고 싶을 때, 처음부터 다시 페인팅할 수 있습니다.

<p align="center">
  <img width="400" alt="페인팅 초기화 기능" src="https://github.com/user-attachments/assets/e5bf596b-9e50-4b80-b06b-81cb26e9e1f4" />
</p>

---

### 5. 결과물 저장 및 서버 전송

페인팅된 텍스처를 파일로 저장하고, `UnityWebRequest`를 활용해 서버로 전송하는 구조를 구현했습니다.  
이를 통해 사용자가 완성한 결과물을 VR 전시장에 반영할 수 있도록 확장했습니다.

<br/>

<table>
  <tr>
    <td align="center" width="50%">
      <b>텍스처 저장 버튼</b><br/>
      <sub>현재 페인팅 결과를 이미지 파일로 저장</sub>
    </td>
    <td align="center" width="50%">
      <b>서버 전송 버튼</b><br/>
      <sub>저장된 결과물을 서버로 업로드</sub>
    </td>
  </tr>
  <tr>
    <td>
      <img width="100%" alt="텍스처 저장 버튼" src="https://github.com/user-attachments/assets/111cfb57-acb3-4181-b301-9ea40af30c71" />
    </td>
    <td>
      <img width="100%" alt="서버 전송 버튼" src="https://github.com/user-attachments/assets/206948b0-2239-4241-bdbe-32f86aafa6b6" />
    </td>
  </tr>
</table>

<br/>

| 기능 | 설명 |
|---|---|
| 텍스처 저장 | 페인팅된 오브젝트의 텍스처를 이미지 파일로 저장 |
| 서버 전송 | `UnityWebRequest`를 활용해 저장된 결과물을 서버로 업로드 |
| 확장 구조 | 업로드된 결과물을 전시장 시스템에서 활용할 수 있도록 연동 가능한 구조로 설계 |
---

## 시스템 흐름

```mermaid
flowchart LR
    A[명화 선택] --> B[VR 페인팅 공간 입장]
    B --> C[색상 선택]
    C --> D[도구 선택]

    subgraph Tools[페인팅 도구]
        E1[페인트 폭탄]
        E2[페인트 건]
        E3[페인트 스프레이]
        E4[페인트 손가락]
    end

    D --> E1
    D --> E2
    D --> E3
    D --> E4

    E1 --> F[핸드트래킹 Grab / Use]
    E2 --> F
    E3 --> F
    E4 --> F

    F --> G[Raycast로 충돌 지점 감지]
    G --> H[textureCoord 기반 UV 좌표 추출]
    H --> I[실시간 텍스처 페인팅]
    I --> J[결과물 저장]
    J --> K[서버 전송 / 전시장 반영]
