# Service Agent Release notes

### 06/03/2023
- Version : 1.3.4
- Code:
- Change history
  - Added an installation date field and APIs to the job data. 
  - Fixed a bug related to certificate validation upon job completion. 
  - Introduced a new feature that allows users to select photos from their phone album. 
  - The serial number-model mapping table has been updated. 
  - Updated the system and packages.


### 28/02/2023
- Version : 1.3.3
- Code:
- Change history
  - Boiler readings for each job are automatically categorized based on the predefined serial number for each boiler type. 
  - Jobs that are updated to CIC now include the boiler readings. 
  - The camera/preview user experience has been updated to allow for picture retakes. 
  - A bug in the signup process has been fixed
  - The system and packages have been updated  
  

### 17/02/2023
- Version : 1.3.2
- Code:
- Change history
  - Estimated symptom updated with symptom name
  - Define a boiler type by Serial Number
  - Display a preset 'Fuel Type' group by the boiler type
  - Set a certificate by the boiler Type
  - Minor bugfix and tweaks
  - System and package updated
  
...

### 29/11/2022
- Version : 1.2.5
- Code:
- Change history
  - services with part-changes updated
  - detail/edit screen updated
  - Tab ordering updated
  - DateTime into UK local time
  - Minor bugfix
  - System and package updated

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
