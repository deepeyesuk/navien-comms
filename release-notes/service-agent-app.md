# Service Agent Release notes

### 14/11/2022
- Version : 1.2.3
- Code:
- Change history
  - Engineer full name and gas safety number added on edit screen
  - 서버쪽에서 CIC API 호출함으로 시스템 안정성을 높임. 모바일에서 CIC API 호출 삭제.
  - PartChanges schema updated on Mobile and APIs, named 'Services'

### 01/11/2022
- Version : 1.2.2
- Code: 22(android), 1(IOS)
- Change history
  - CIC/Mobile APIs updated
  - Additional fields (estimatedSymptom, customerComments, etc) added
  - Part-changes updated
  - Validation message bugfix
  - System and package updated

- Communication


### 25/10/2022
- Version : 1.2.1
- Code: 21(android), 1(IOS)
- Change history
  - Validation enhanced
  - Alert messages on Job Status
  - Part-change controllers updated
  - APIs refined
  - System updated and Minor bugfixs

- Communication
  - CIC에서 Create Job API 콜을 통해 CIC로부터 받아오는 정보들 수정 불가
  - 완료된 (Completed) 작업은 수정 불가능
  - 교체 부품 등록, Part Change에서 +, - 사인으로만 Quantity를 조정함으로 숫자이외의 글자 입력 금지
  - Job 배정시 바로 Complete 되지 않도록 Symptom, Repair, Serial Number 등의 필수 항목이 없을 경우, 완료 (Completed)가 되지 않도록 검증
  - 이미 완료된 작업 재 완료 불가능
