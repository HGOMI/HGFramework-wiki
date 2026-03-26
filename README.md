# HGFramework

- HGFramework는 멀티모듈 구조로 구성된 Paper/Spigot `1.21.11` 서버용 통합 플러그인입니다.


## 구현된 시스템

- 메뉴 허브 시스템: `Shift + F`로 6x9 메인 메뉴 오픈, 기능 버튼 확장 가능
  - `config.yml > menu-hub.buttons.*`에서 버튼별 표시/숨김, 다중 슬롯 배치, 위치 커스텀 지원
- 전투 제어 시스템: 설정에 등록된 아이템으로만 공격 허용
- 인벤세이브권 시스템: 사망 시 티켓 1개 소모 후 인벤토리 보존
- 성장형 가방 시스템: 활성화권/줄 확장권/페이지 확장권, 최대 3페이지
- 재화 시스템: `원` 단위, 액션바 지갑 표시, 한글 금액 포맷, Vault 연동, 유저 간 송금(토글 가능)
- 유저 거래소 시스템: P2P 등록/구매/검색/수수료/정산
- 서버 상점 시스템: 관리자 등록 상품 구매/판매
- 워프 시스템:
  - 워프 포인트 생성/좌표생성, 활성/비활성, 비용 설정
  - 워프 GUI 슬롯 고정 배치, 워프별 아이콘(Material/CMD) 커스텀
  - 카운트다운 후 워프(이동/피격/밀림/텔포/사망/로그아웃 시 취소)
  - 카운트다운 표시 위치 설정(ACTION_BAR/CHAT/TITLE/NONE)
- 아이템 강화 시스템:
  - 강화 대상/강화석/하락 방어/파괴 방지 슬롯을 갖는 전용 GUI
  - 단계별 성공/실패 확률 + 실패 시 유지/하락/파괴 확률 분기
  - 강화석 확률 보정, 주문서 발동 시 하락/파괴를 유지로 전환
  - 단계별 강화 비용(원) 차감, 트랙/확률/특수 아이템 yml 기반 운영
- 아이템 제작 시스템:
  - 제작 목록 GUI + 상세 GUI(좌측 재료/우측 결과/중앙 제작 버튼)
  - 재료/비용 검증 후 제작, 부족 시 차감 없이 실패 처리
  - 레시피별 활성 상태, 가격, 슬롯, 재료/결과/아이콘 관리
  - 제작 전용 yml(`settings.yml`, `recipes.yml`) 기반 운영
- 멀티월드/인스턴스 시스템:
  - 월드 생성/삭제/설정
  - 템플릿 등록(기존 월드/폴더)
  - 인스턴스 생성/입장/퇴장/삭제/정보/정리
  - TTL 자동 정리, 시작 시 비정상 정리, 생성 큐(FIFO)
- 도감 시스템:
  - 관리자 도감 항목 편집 GUI
  - 유저 아이템 제출/완료 처리, 완료 상태 표시
  - 완료 개수 마일스톤 보상(돈/아이템), 플레이어별 수령 이력 관리
- 닉네임 시스템:
  - 관리자 닉네임 변경/초기화
  - 닉네임변경권 사용(우클릭 + 채팅 입력)
  - 길이/정규식/금지어 검증, 중복 닉네임 방지
  - 머리 위 이름/탭리스트 반영, 명령 대상 해석/탭완성 연동
- 아이템 금지 시스템:
  - 금지 타입 4종: 사용/설치/착용/소지
  - 전역/월드별 규칙 분리 적용
  - 관리자 편의 명령(메인핸드 아이템 기준 등록/해제/조회)
  - 관리자 우회 권한 지원
- 사유지(Estate) 시스템:
  - 영지 코어: 생성/편집/점유/해제, 역할/플래그 기반 보호, 초대/신뢰/밴, 지도/경계/스폰
  - 영지 경제: 은행/세금/유지비/정산/원장(ledger) + 복구(recovery)
  - 구역/거래: 3D 구역, 임대/판매 오퍼, 계약 만료/해지
  - 국가/전쟁: 국가 생성/외교/수도, 국가 설정 GUI(이름변경/해체 확인), 전쟁 선언/수락/점령/항복/정산/HUD
  - 사유지/국가 GUI 슬롯 커스텀: 영지(`estate-core/menus.yml`), 국가 엔트리/메인/설정/해체확인(`estate-nation/nation-settings.yml`)
  - 디스코드 연동: 계정 링크, 이벤트 전송 큐+DLQ, 티켓, 영지/국가 채널 매핑, 디코→마크 채팅 폴링, 국가명 변경 시 채널/역할명 동기화
  - 디코 동기화 성능: 영지/국가 채널·역할 생성/삭제/이름변경 비동기 큐 처리(생성/삭제 순간 멈칫 완화)
  - 디스코드 GUI 슬롯 커스텀: 유저 메뉴/관리자 메뉴(`config.yml > discord-menu.*`)
- 런타임 리로드 시스템: `/hgreload`로 서버 재시작 없이 설정/모듈 재적용

## 명령어 사용법

- `/돈 [플레이어]`: 지갑 잔액 확인
- `/돈 상태`: 현재 경제 모드/백엔드/Vault provider 상태 확인(관리자)
- `/돈지급 [플레이어] [금액]`: 재화 지급
- `/돈차감 [플레이어] [금액]`: 재화 차감
- `/돈설정 [플레이어] [금액]`: 재화 설정
- `/돈전달 [플레이어] [금액]` (`/송금`): 유저 간 돈 전달 (온라인 대상)

- `/거래소`: 메인 거래소 UI 열기
- `/내상점`: 내 상점 관리 UI 열기

- `/서버상점`: 서버 상점 UI 열기
- `/상점등록 [ID] [구매가] [판매가(선택)]`: 손에 든 아이템을 해당 ID로 등록
- `/상점삭제 [ID]`: 해당 ID 상품 삭제
- `/상점가격 [ID] [구매/판매] [가격]`: 가격 변경
- `/상점활성 [ID] [on/off]`: 상품 활성 상태 변경

- `/인벤세이브권 [플레이어] [수량]`: 인벤세이브권 지급

- `/가방지급 [플레이어]`: 가방 활성화권 지급
- `/가방확장 [플레이어] [줄/페이지]`: 가방 확장권 지급
- `/가방초기화 [플레이어]`: 가방 데이터 초기화

- `/월드 <생성|삭제|목록|이동|설정>`: 월드 관리
- `/템플릿 <등록|등록폴더|목록|삭제>`: 템플릿 관리
- `/인스턴스 <생성|입장|퇴장|삭제|목록|정보|정리>`: 인스턴스 관리

- `/워프 [워프ID]`: 워프 GUI 열기 / 특정 워프 카운트다운 시작
- `/워프관리 생성 <id> [이름]`: 현재 위치 기반 워프 생성(기본 비활성)
- `/워프관리 좌표생성 <id> <이름> <world> <x> <y> <z> [yaw] [pitch]`: 좌표 기반 워프 생성
- `/워프관리 활성 <id> <on/off>`: 워프 활성/비활성
- `/워프관리 비용 <id> <금액>`: 워프 이용 비용 설정
- `/워프관리 슬롯 <id> <slot,slot,...>`: 워프 GUI 슬롯 설정(0~53, 다중 가능)
- `/워프관리 아이콘 <id> <material> [customModelData]`: 워프 아이콘 설정
- `/워프관리 이름 <id> <표시이름>`: 워프 표시 이름 변경
- `/워프관리 삭제 <id>`: 워프 삭제
- `/워프관리 목록`: 워프 목록 조회
- `/워프리로드`: 워프 시스템/warps.yml 재로드

- `/강화`: 강화 GUI 열기
- `/강화테스트 [플레이어]`: 강화 GUI 테스트 오픈
- `/강화리로드`: 강화 시스템 yml 재로드
- `/강화관리 강화석보정 <stoneId> <성공확률보정값>`: 강화석 보정값 수정
- `/강화관리 강화석목록`: 강화석 ID/보정값 목록 조회
- `/강화관리 등록 <trackId> <level> <cost> [none]`: 메인핸드=input-item, 오프핸드=next-item(또는 `none`)
- `/강화관리 삭제 <trackId> <level>`: 트랙 특정 레벨 삭제
- `/강화관리 트랙삭제 <trackId>`: 트랙 전체 삭제
- `/강화석지급 <stoneId> [플레이어] [수량]`: 강화석 지급
- `/하락방어지급 <guardId> [플레이어] [수량]`: 하락 방어 주문서 지급
- `/파괴방지지급 <guardId> [플레이어] [수량]`: 파괴 방지 주문서 지급

- `/제작 [페이지]`: 제작 메인 GUI 열기
- `/제작리로드`: 제작 시스템 yml 재로드
- `/제작관리 목록 [페이지]`: 제작 레시피 목록 조회
- `/제작관리 등록 <id> <가격> [결과수량]`: 메인핸드 아이템 기준 레시피 등록
- `/제작관리 삭제 <id>`: 레시피 삭제
- `/제작관리 활성 <id> <on/off>`: 레시피 활성/비활성
- `/제작관리 가격 <id> <금액>`: 레시피 비용 변경
- `/제작관리 슬롯 <id> <slot,slot,...>`: 메인 GUI 슬롯 배치
- `/제작관리 결과설정 <id> [수량]`: 메인핸드 아이템으로 결과 설정
- `/제작관리 아이콘설정 <id>`: 메인핸드 아이템으로 아이콘 설정
- `/제작관리 재료추가 <id> <수량>`: 메인핸드 아이템을 재료로 추가
- `/제작관리 재료삭제 <id> <index>`: 재료 인덱스 삭제
- `/제작관리 재료초기화 <id>`: 재료 목록 초기화

- `/도감`: 도감 UI 열기
- `/도감관리 [페이지]`: 도감 항목 관리 GUI 열기
- `/도감보상관리 [완료개수] [돈|아이템|삭제] [값]`: 도감 마일스톤 보상 관리
- `/도감리로드`: 도감 yml 재로드(`entries.yml`, `rewards.yml`)
- `/도감초기화 [플레이어]`: 플레이어 도감 진행/보상 이력 초기화

- `/닉변경 [플레이어] [새닉네임]`: 관리자 닉네임 변경
- `/닉초기화 [플레이어]`: 관리자 닉네임 초기화
- `/닉변권지급 [플레이어] [수량]`: 닉네임변경권 지급
- `/닉리로드`: 닉네임 시스템/금지어 리로드

- `/아이템금지 [전역|월드 <월드명>] [사용|설치|착용|소지|전체] [on/off]`: 메인핸드 아이템 금지 설정
- `/아이템금지해제 [전역|월드 <월드명>]`: 메인핸드 아이템 금지 설정 삭제
- `/아이템금지목록 [전역|월드 <월드명>] [페이지]`: 금지 목록 조회
- `/아이템금지리로드`: 아이템 금지 시스템 리로드

- `/영지 [도움|생성|삭제|편집|목록|정보|선택|점유|해제|초대|초대목록|수락|거절|신뢰|신뢰해제|밴|밴해제|역할설정|역할|개인설정|은행|세금|유지비|구역|임대|판매|스폰설정|스폰|경계보기|지도]`: 사유지 기본 명령
- `/영지관리 <리로드 [저장]|영지목록|영지정보|영지삭제|소유자변경|플레이어영지|플래그동기화|야생설정|야생플래그|기본역할 동기화|플래그리셋|명령제한 리로드|경제 ...|구역 ...|임대 ...|판매 ...>`: 사유지 관리자 명령
- `/국가 <gui [국가명]|도움|생성|삭제 [국가명]|이름변경 [국가명] <새이름>|정보 [국가명] [gui]|목록 [gui|페이지]|초대|초대목록 [gui|페이지]|가입수락|가입거절|탈퇴|추방|수도|관계|리로드>`: 국가 시스템 명령
  - `/국가 삭제`는 즉시 삭제가 아니라 GUI 해체 확인창을 엽니다.
- `/전쟁 <선포|선언목록 [gui|페이지]|수락|거절|항복|점령|정보 <warId> [gui]|상황 <warId>|목록 [페이지|gui]>`: 전쟁 시스템 명령
- `/전쟁관리 <리로드|강제시작|강제종료 [TIMEOUT|ADMIN_FORCE|MIN_PLAYERS_NOT_MET]|로그>`: 전쟁 관리자 명령
- `/디코연동 <상태|링크 [코드 디스코드ID]|해제|티켓 ...|관리 ...>`: 디스코드 연동 명령

- `/hgreload`: 플러그인 설정/모듈 리로드

## 권한 요약

- `hgframework.admin.reload`: `/hgreload`
- `hgframework.admin.money`: 재화 관리자 명령
- `hgframework.admin.ticket`: 인벤세이브권 지급
- `hgframework.admin.bag`: 가방 관리자 명령
- `hgframework.admin.market`: 거래소 관리자 권한
- `hgframework.admin.servershop`: 서버상점 관리자 명령
- `hgframework.admin.world`: 월드 관리자 명령
- `hgframework.admin.template`: 템플릿 관리자 명령
- `hgframework.admin.instance`: 인스턴스 전체 관리자 권한
- `hgframework.admin.instance.create|join|leave|delete|list|info|cleanup`: 인스턴스 서브 권한
- `hgframework.admin.collection`: 도감 관리 권한(`/도감관리`)
- `hgframework.admin.collection.reward`: 도감 보상 관리 권한(`/도감보상관리`)
- `hgframework.admin.collection.reload`: 도감 리로드 권한(`/도감리로드`)
- `hgframework.admin.collection.reset`: 도감 초기화 권한(`/도감초기화`)
- `hgframework.admin.nickname`: 닉네임 관리 권한(`/닉변경`, `/닉초기화`)
- `hgframework.admin.nickname.ticket`: 닉네임변경권 지급 권한(`/닉변권지급`)
- `hgframework.admin.nickname.reload`: 닉네임 리로드 권한(`/닉리로드`)
- `hgframework.admin.itemrestriction.manage`: 아이템 금지 관리 권한
- `hgframework.admin.itemrestriction.reload`: 아이템 금지 리로드 권한
- `hgframework.admin.itemrestriction.bypass`: 아이템 금지 우회 권한
- `hgframework.warp.use`: 워프 사용 권한
- `hgframework.admin.warp.manage`: 워프 관리 권한
- `hgframework.admin.warp.reload`: 워프 리로드 권한
- `hgframework.warp.bypass.cost`: 워프 비용 차감 우회 권한
- `hgframework.warp.bypass.countdown`: 워프 카운트다운 우회 권한
- `hgframework.enhancement.use`: 강화 사용 권한
- `hgframework.admin.enhancement.test`: 강화 테스트 GUI 권한
- `hgframework.admin.enhancement.manage`: 강화 관리/지급 권한
- `hgframework.admin.enhancement.reload`: 강화 리로드 권한
- `hgframework.crafting.use`: 제작 사용 권한
- `hgframework.admin.crafting.manage`: 제작 관리 권한(`/제작관리`)
- `hgframework.admin.crafting.reload`: 제작 리로드 권한(`/제작리로드`)
- `hgframework.estate.use`: 사유지 명령 기본 사용 권한
- `hgframework.admin.estate.manage`: 사유지 관리자 루트 권한
- `hgframework.admin.estate.economy.*`: 사유지 경제 관리자 권한
- `hgframework.estate.area.*`: 구역/임대/판매 사용 권한
- `hgframework.admin.estate.area.manage`: 구역/임대/판매 관리자 권한
- `hgframework.estate.nation.use|manage`: 국가 시스템 권한
- `hgframework.admin.estate.nation.manage`: 국가 관리자 권한
- `hgframework.estate.war.use|manage`: 전쟁 시스템 권한
- `hgframework.admin.estate.war.manage`: 전쟁 관리자 권한
- `hgframework.estate.discord.use|link`: 디스코드 연동/링크 권한
- `hgframework.admin.estate.discord.manage`: 디스코드 연동 관리자 권한

## 주요 설정

- 경제 백엔드 모드:
  - `config.yml` -> `currency.mode = internal|vault|disabled`
  - `internal`: HGFramework 내부 지갑
  - `vault`: Vault 외부 Economy 제공자 사용
  - `disabled`: 돈 기능 비활성
- Vault provider 미탐지 정책:
  - `config.yml` -> `currency.vault.on-provider-missing = fallback-disabled|fail-fast`
  - `fallback-disabled`: 서버는 계속 동작, 돈 기능만 비활성 폴백
  - `fail-fast`: 서버 시작/리로드 실패 처리
- 레거시 호환:
  - `currency.mode` 미설정 시 `currency.enabled` 값을 자동 추론(경고 로그 출력)
- 유저 간 송금 on/off: `config.yml` -> `currency.player-transfer.enabled`
- 닉네임 시스템 on/off: `config.yml` -> `nickname.enabled`
- 닉네임 정책: `config.yml` -> `nickname.policy.*`
- 닉네임 금지어 파일: `plugins/HGFramework/nickname-system/blocked-words.yml`
- 아이템 금지 시스템 on/off: `config.yml` -> `item-restriction.enabled`
- 아이템 금지 동작 정책: `config.yml` -> `item-restriction.carry-action`, `item-restriction.message-cooldown-millis`
- 아이템 금지 규칙 파일: `plugins/HGFramework/item-restriction-system/restrictions.yml`
- 워프 시스템 on/off: `config.yml` -> `warp.enabled`
- 메뉴 허브 버튼 커스텀: `config.yml` -> `menu-hub.buttons.<buttonId>.enabled|slot|slots`
- 디코 연동 GUI 슬롯 커스텀: `config.yml` -> `discord-menu.user.slots.*`, `discord-menu.admin.slots.*`
- 서버 관리 GUI 버튼 커스텀(관리자 전용): `config.yml` -> `menu-hub.buttons.server-admin.*`
- 워프 카운트다운/취소 정책: `config.yml` -> `warp.countdown-seconds`, `warp.countdown-display`, `warp.cancel-movement-threshold`
- 워프 권한/우회: `config.yml` -> `warp.use-permission`, `warp.manage-permission`, `warp.bypass-cost-permission`, `warp.bypass-countdown-permission`
- 워프 데이터 파일: `plugins/HGFramework/warp-system/warps.yml`
- 강화 시스템 설정: `plugins/HGFramework/enhancement-system/settings.yml` (`enabled`, `permissions.*`, `gui.*`, `messages.*`)
- 강화 확률 파일: `plugins/HGFramework/enhancement-system/probabilities.yml` (`levels.<강화단계>.success`, `failure.keep/down/destroy`)
- 강화 트랙 파일: `plugins/HGFramework/enhancement-system/tracks.yml` (`tracks.<trackId>.levels.<level>.input-item/next-item/cost`)
- 강화 특수아이템 파일: `plugins/HGFramework/enhancement-system/special-items.yml` (`stones`, `down-guards`, `destroy-guards`)
- 제작 시스템 설정: `plugins/HGFramework/crafting-system/settings.yml` (`enabled`, `permissions.*`, `gui.*`, `messages.*`)
- 제작 레시피 파일: `plugins/HGFramework/crafting-system/recipes.yml` (`recipes.<id>.*`)
- 사유지 코어 설정/데이터: `plugins/HGFramework/estate-core/*.yml` (`menus.yml`의 `main.slots.*`, `members.content-slots`, `roles.content-slots`, `flags.*`)
- 사유지 경제 설정/데이터: `plugins/HGFramework/estate-economy/*.yml`
- 사유지 구역/임대/판매 설정/데이터: `plugins/HGFramework/estate-area/*.yml`
- 국가 설정/데이터: `plugins/HGFramework/estate-nation/*.yml` (`nation-settings.yml`의 `gui.entry|main|settings|delete-confirm.slots.*`)
- 전쟁 설정/데이터: `plugins/HGFramework/estate-war/*.yml`
- 디스코드 연동 설정/데이터: `plugins/HGFramework/estate-discord/*.yml`

## 의존성

- 필수: Vault
- 선택: PlaceholderAPI
  - 재화: `%hgframework_balance%`, `%hgframework_balance_formatted%`, `%hgframework_wallet%`, `%hgframework_wallet_formatted%`
  - 닉네임: `%hgnickname_nickname%`, `%hgnickname_display%`, `%hgnickname_realname%`, `%hgnickname_has_nickname%`
