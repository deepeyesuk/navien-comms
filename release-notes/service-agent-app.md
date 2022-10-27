# Service Agent Release notes

### 25/10/2022 (expected)
- Version : 1.2.1
- Code: 21(android), 1(IOS)
- Change history
  - Validation enhanced
  - Alert messages on Job Status
  - Part-change controllers updated
  - APIs refined

- Communication
  - CIC에서 Create Job API 콜을 통해 CIC로부터 받아오는 정보들 수정 불가
  - 완료된 (Completed) 작업은 수정 불가능
  - 교체 부품 등록, Part Change에서 +, - 사인으로만 Quantity를 조정함으로 숫자이외의 글자 입력 금지
  - Job 배정시 바로 Complete 되지 않도록 Symptom, Repair, Serial Number 등의 필수 항목이 없을 경우, 완료 (Completed)가 되지 않도록 검증
  - 이미 완료된 작업 재 완료 불가능
