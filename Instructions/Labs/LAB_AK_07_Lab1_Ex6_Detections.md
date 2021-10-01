# ���W���[�� 7 - ���{ 1 - ���K 6 - ���o���쐬����

### �^�X�N 1: Sysmon �ɂ��U��1�̌��o

���̃^�X�N�ł́A�Z�L�����e�B�C�x���g�R�l�N�^�� Sysmon ���C���X�g�[������Ă���z�X�g�ōU�� 1 �̌��o���쐬���܂��B

���̍U���ɂ��A�N�����Ɏ��s����郌�W�X�g���L�[���쐬����܂��B  
```Command
REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /V "SOC Test" /t REG_SZ /F /D "C:\temp\startup.bat"
```

1. �Ǘ��҂Ƃ��� WIN1 ���z�}�V���Ƀ��O�C�����܂��B�p�X���[�h�� **Pa55w.rd** �ł��B  

2. Edge �u���E�U�[�� Azure �|�[�^���Ɉړ����܂� https://portal.azure.com

3. **�T�C���C��**�_�C�A���O�{�b�N�X�ŁA���{�̃z�X�e�B���O�v���o�C�_�[����񋟂��ꂽ�Ǘ��җp��**�e�i���g�d�q���[��**�A�J�E���g���R�s�[���ē\��t���A**����**��I�����܂��B

4. **�p�X���[�h�̓���**�_�C�A���O �{�b�N�X�ŁA���{ �z�X�e�B���O �v���o�C�_�[����񋟂��ꂽ�Ǘ��җp��**�e�i���g�p�X���[�h** ���R�s�[���ē\��t���A**�T�C���C��**��I�����܂��B

5. Azure �|�[�^���̌����o�[�� �u*Sentinel*�v �Ɠ��͂��A�u**Azure Sentinel**�v ��I�����܂��B

6. ��قǍ쐬���� Azure Sentinel ���[�N�X�y�[�X��I�����܂��B

7. ��ʃZ�N�V�������� **Logs** ��I�����܂��B

8. �܂��A�f�[�^���ۑ�����Ă���ꏊ���m�F����K�v������܂��B�U�����s�����΂���Ȃ̂�  ���O�̎��Ԕ͈͂�**�ߋ� 24 ����**�ɐݒ肵�܂��B

9. ���� KQL �X�e�[�g�����g�����s���܂�

```KQL
search "temp\\startup.bat"
```

10. ���ʂ́A3�̈قȂ�e�[�u���ɂ��Ď����Ă��܂��B
    - DeviceProcessEvents
    - DeviceRegistryEvents
    - Event

    *�f�o�C�X* �e�[�u���́ADefender for Endpoint (�f�[�^�R�l�N�^ - Microsoft 365 Defender) ����̂��̂ł��B  *�C�x���g*�́A�f�[�^ �R�l�N�^�̃Z�L�����e�B �C�x���g����̂��̂ł��B 

    Sysmon �� Defender for Endpoint �� 2 �̈قȂ�\�[�X����f�[�^����M���Ă��邽�߁A��Ō����ł��� 2 �� KQL �X�e�[�g�����g���쐬����K�v������܂��B  �ŏ��̒����ł́A���ꂼ����ʂɊm�F���Ă����܂��B

    **��:** �܂�ɁA�f�[�^�̓ǂݍ��݃v���Z�X�̓ǂݍ��݂ɒʏ�������Ԃ�������ꍇ������܂��B  ���̏ꍇ�A�e�[�u�����N�G���ɐ����ԕ\������Ȃ����Ƃ�����܂��B

11. �ŏ��̃f�[�^�\�[�X�́AWindows �z�X�g����� Sysmon �ł��B  �ȉ��� KQL �X�e�[�g�����g�����s���܂��B

```KQL
search in (Event) "temp\\startup.bat"
```
���ʂ́A�C�x���g�e�[�u���ɑ΂��Ă̂ݕ\�������悤�ɂȂ�܂����B  

12. �s��W�J���āA���R�[�h�Ɋ֘A���邷�ׂĂ̗��\�����܂��B  EventData �� ParameterXml �Ȃǂ̂������̃t�B�[���h�ɂ́A�\�����f�[�^�Ƃ��ĕۑ����ꂽ�����̃f�[�^���ڂ�����܂��B  ����ɂ��A����̃t�B�[���h�ł̃N�G��������ɂȂ�܂��B  

13. ���ɁA�e�s�̃f�[�^����͂��� KQL �X�e�[�g�����g���쐬���āA�Ӗ��̂���t�B�[���h���쐬����K�v������܂��B  GitHub �� Azure Sentinel �R�~���j�e�B�ł́AParsers �t�H���_�[�� Parsers �̗Ⴊ��������܂��B  �u���E�U�[�ŐV�����^�u���J���Ahttps://github.com/Azure/Azure-Sentinel �Ɉړ����܂�

14. **Parsers**�t�H���_�[��I�����A���� **Sysmon** �t�H���_�[��I�����܂��B  �ȉ��̂��̂�������͂��ł��BAzure-Sentinel/Parsers/Sysmon/Sysmon-v12.0.txt

15. Sysmon-v12.0.txt �t�@�C����I�����m�F���܂��B

�t�@�C���̐擪�ɁAEvent �e�[�u�����N�G�����AEventData �Ƃ������O�̕ϐ��Ɋi�[���� Let �X�e�[�g�����g���\������܂��B


```KQL
let EventData = Event
| where Source == "Microsoft-Windows-Sysmon"
| extend RenderedDescription = tostring(split(RenderedDescription, ":")[0])
| project TimeGenerated, Source, EventID, Computer, UserName, EventData, RenderedDescription
| extend EvData = parse_xml(EventData)
| extend EventDetail = EvData.DataItem.EventData.Data
| project-away EventData, EvData  ;
```

�t�@�C���̂���ɉ��ɁAEventID == 13 �𒲂ׁAEventData �ϐ�����͂Ƃ��Ďg�p���Ă���ʂ� let �X�e�[�g�����g������܂��B  

```KQL
let SYSMON_REG_SETVALUE_13=()
{
    let processEvents = EventData
    | where EventID == 13
    | extend RuleName = EventDetail.[0].["#text"], EventType = EventDetail.[1].["#text"], UtcTime = EventDetail.[2].["#text"], ProcessGuid = EventDetail.[3].["#text"], 
    ProcessId = EventDetail.[4].["#text"], Image = EventDetail.[5].["#text"], TargetObject = EventDetail.[6].["#text"], Details = EventDetail.[7].["#text"]
    | project-away EventDetail  ;
    processEvents;
    
};
```
����͗ǂ��X�^�[�g�̂悤�Ɍ����܂��B

16. ��L�̃X�e�[�g�����g���g�p���ēƎ��� KQL �X�e�[�g�����g���쐬���A���ׂẴ��W�X�g���L�[�Z�b�g�l�̍s��\�����܂��B  ���� KQL �N�G�������s���܂��B

```KQL

Event
| where Source == "Microsoft-Windows-Sysmon"
| where EventID == 13
| extend RenderedDescription = tostring(split(RenderedDescription, ":")[0])
| project TimeGenerated, Source, EventID, Computer, UserName, EventData, RenderedDescription
| extend EvData = parse_xml(EventData)
| extend EventDetail = EvData.DataItem.EventData.Data
| project-away EventData, EvData  
| extend RuleName = EventDetail.[0].["#text"], EventType = EventDetail.[1].["#text"], UtcTime = EventDetail.[2].["#text"], ProcessGuid = EventDetail.[3].["#text"], 
    ProcessId = EventDetail.[4].["#text"], Image = EventDetail.[5].["#text"], TargetObject = EventDetail.[6].["#text"], Details = EventDetail.[7].["#text"]
    | project-away EventDetail 


```

   ![�X�N���[���V���b�g](../Media/SC200_sysmon_query1.png)

17.  ������������������o���[�����쐬�ł��܂����A����KQL�X�e�[�g�����g�́A���̌��o���[����KQL�X�e�[�g�����g�ōė��p�ł���悤�Ɍ����܂��B  �u���O�v�E�B���h�E�ŁA�u**�ۑ�**�v�A�u**�֐��Ƃ��ĕۑ�**�v�̏��ɑI�����܂��B�u�ۑ��v �t���C�A�E�g�ŁA���̂悤�ɓ��͂��Ċ֐���ۑ����܂��B

�֐���: Event_Reg_SetValue
�J�e�S��: Sysmon


18. �V���� �u���O �N�G���v �^�u���J���܂��B�����āA�ȉ��� KQL �X�e�[�g�����g�����s���܂�:

```KQL

Event_Reg_SetValue

```
�݂̃f�[�^���W�ɂ���ẮA�����̍s���󂯎��\��������܂��B  ����͗\������Ă��邱�Ƃł��B  ���̃^�X�N�́A����̃V�i���I�Ƀt�B���^�[�������邱�Ƃł�

19. �ȉ��� KQL �X�e�[�g�����g�����s���܂�:

```KQL

Event_Reg_SetValue | search "startup.bat"

```
����ɂ��A����̃��R�[�h���Ԃ���A�f�[�^���m�F���āA�s�����ʂ��邽�߂ɉ���ύX�ł��邩���m�F�ł��܂�

20. ���ЃC���e���W�F���X����A���ЃA�N�^�[�� reg.exe ���g�p���ă��W�X�g���L�[��ǉ����Ă��邱�Ƃ��킩��܂��B  �f�B���N�g���� c:\temp. �ł��Bstartup.bat �͕ʂ̖��O�ɂ��邱�Ƃ��ł��܂��B���̃X�N���v�g�����s���܂��B

```KQL
Event_Reg_SetValue 
| where Image contains "reg.exe"

```
����͗ǂ��X�^�[�g�ł�  ���ɁAc:\temp �f�B���N�g���̌��ʂ݂̂�Ԃ��K�v������܂��B

21. �����āA�ȉ��� KQL �X�e�[�g�����g�����s���܂�:

```KQL
Event_Reg_SetValue 
| where Image contains "reg.exe"
| where Details startswith "C:\\TEMP"
```

����͗ǂ����o���[���̂悤�Ɍ����܂��B  

22. �A���[�g�ɂ��Ăł��邾�������̃R���e�L�X�g��񋟂��邱�Ƃɂ��A�Z�L�����e�B�^�p�A�i���X�g���x�����邱�Ƃ��d�v�ł��B����ɂ́A�����O���t�Ŏg�p����G���e�B�e�B�̓��e���܂܂�܂��B  ���̃N�G�������s���܂��B

```KQL
Event_Reg_SetValue 
| where Image contains "reg.exe"
| where Details startswith "C:\\TEMP"
| extend timestamp = TimeGenerated, HostCustomEntity = Computer, AccountCustomEntity = UserName

```

23. �K�؂Ȍ��o���[�����ł����̂ŁA�N�G���̂��郍�O�E�B���h�E�ŁA�R�}���h �o�[�� **�u�V�����A���[�g ���[���v** ��I�����A**�uAzure Sentinel �A���[�g�̍쐬�v** ��I�����܂��B

24. ����ɂ��A�A�i���e�B�N�X���[���E�B�U�[�h���N�����܂��B  �S�ʃ^�u�Ɏ��̂悤�ɓ��͂��܂�

    ����: Sysmon Startup RegKey

    ����: Sysmon Startup Regkey in c:\temp

    �^�N�e�B�N�X: �i����

    �d��x: ��

�u**����: ���[�� ���W�b�N�̐ݒ� >**�v��I�����܂��B

25. �u**���[�� ���W�b�N�̐ݒ�**�v �^�u�ŁA**���[�� �N�G��**�����ɓ��͂���Ă���͂��ł��B 

26. �N�G���X�P�W���[�����O�̏ꍇ�A���̂悤�ɐݒ肵�܂��B

- ������x�N�G�������s����: 5 ��
- �Ōォ��̃f�[�^�����Ă��������F  1 ��

**��** �����f�[�^�ɑ΂��ĈӐ}�I�ɑ����̃C���V�f���g�𐶐����Ă��܂��B  ����ɂ��A���{�͂����̃A���[�g���g�p�ł���悤�ɂȂ�܂��B

27. �c��̃I�v�V�����͊���l�̂܂܂ɂ��܂��B  �u**����: �C���V�f���g�ݒ� >**�v�{�^����I�����܂��B

28. �C���V�f���g�ݒ�ɂ́A�ȉ���ݒ肵�܂� 

- �C���V�f���g�̐ݒ�F �L��
- �A���[�g �O���[�v�F ����

�u**����: �������� >**�v�{�^����I�����܂��B

29. ���������^�u�Ŏ��̂悤�ɐݒ肵�܂��B

- *PostMessageTeams-OnAlert* ��I�����܂��B

�u**����: ���r���[**�v�{�^����I�����܂��B

30. ���r���[�^�u�ŁA**�쐬**��I�����܂��B


### �^�X�N 2: �G���h�|�C���g�� Defender �ɂ��U��1�̌��o

���̃^�X�N�ł́AMicrosoft Defender for Endpoint ���\�����ꂽ�z�X�g�ōU��1�̌��o���쐬���܂��B

���̍U���ɂ��A�N�����Ɏ��s����郌�W�X�g���L�[���쐬����܂��B  
```Command
REG ADD "HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run" /V "SOC Test" /t REG_SZ /F /D "C:\temp\startup.bat"
```

1. Azure Sentinel �|�[�^���ŁA�S�ʃZ�N�V��������**���O**��I�����܂��B

2. �܂��A�f�[�^���ۑ�����Ă���ꏊ���m�F����K�v������܂��B�U�����s�����΂���Ȃ̂�  

    ���O�̎��Ԕ͈͂��ߋ� 24 ���Ԃɐݒ肵�܂��B

3. �ȉ��� KQL �X�e�[�g�����g�����s���܂�:

```KQL
search "temp\\startup.bat"
```

4. ���ʂ́A3�̈قȂ�e�[�u���ɂ��Ď����Ă��܂��B
    DeviceProcessEvents
    DeviceRegistryEvents
    �C�x���g

    �f�o�C�X*�e�[�u���́ADefender for Endpoint �i�f�[�^�R�l�N�^-Microsoft 365 Defender�j ����̂��̂ł��B  �C�x���g�́A�f�[�^�R�l�N�^�̃Z�L�����e�B�C�x���g����̂��̂ł��B 

    Sysmon �� Defender for Endpoint �� 2 �̈قȂ�\�[�X����f�[�^����M���Ă��邽�߁A  ��Ō����ł��� 2 �� KQL �X�e�[�g�����g���쐬����K�v������܂��B  �������A�ŏ��̒����ł́A���ꂼ����ʂɊm�F���Ă����܂��B

5. ���̌��o�́ADefender for Endpoint ����̃f�[�^�ɏœ_�𓖂Ă܂��B  �ȉ��� KQL �X�e�[�g�����g�����s���܂�:

```KQL
search in (Device*) "temp\\startup.bat"
```

6. �e�[�u�� - DeviceRegistryEvents �́A�f�[�^�����łɐ��K������Ă���A�N�G�����ȒP�ɂł���悤�Ɍ����܂��B  �s��W�J���āA���R�[�h�Ɋ֘A���邷�ׂĂ̗��\�����܂��B

7. ���ЃC���e���W�F���X����A���ЃA�N�^�[�� reg.exe ���g�p���ă��W�X�g���L�[��ǉ����Ă��邱�Ƃ��킩��܂��B  �f�B���N�g���� c:\temp �ł��Bstartup.bat �͕ʂ̖��O�ɂ��邱�Ƃ��ł��܂��B  ���� KQL �X�e�[�g�����g����͂��܂��B

```KQL

DeviceRegistryEvents
| where ActionType == "RegistryValueSet"
| where InitiatingProcessFileName == "reg.exe"
| where RegistryValueData startswith "c:\\temp"


```

����͗ǂ����o���[���̂悤�Ɍ����܂��B  

8. �A���[�g�ɂ��Ăł��邾�������̃R���e�L�X�g��񋟂��邱�Ƃɂ��A�Z�L�����e�B�I�y���[�V�����Z���^�[�A�i���X�g���x�����邱�Ƃ��d�v�ł��B����ɂ́A�����O���t�Ŏg�p����G���e�B�e�B�̓��e���܂܂�܂��B���̃N�G�������s���܂��B

```KQL
DeviceRegistryEvents
| where ActionType == "RegistryValueSet"
| where InitiatingProcessFileName == "reg.exe"
| where RegistryValueData startswith "c:\\temp"
| extend timestamp = TimeGenerated, HostCustomEntity = DeviceName, AccountCustomEntity = InitiatingProcessAccountName


```

   ![�X�N���[���V���b�g](../Media/SC200_sysmon_query2.png)

9.  �K�؂Ȍ��o���[�����ł����̂ŁA�N�G���̂��郍�O �E�B���h�E�ŁA�R�}���h �o�[�� **�u�V�����A���[�g ���[���v** ��I�����܂��B  ���ɁA**�uAzure Sentinel �A���[�g�̍쐬�v** ��I�����܂��B

10. ����ɂ��A�A�i���e�B�N�X���[���E�B�U�[�h���N�����܂��B  �S�ʃ^�u�Ɏ��̂悤�ɓ��͂��܂�


    ����: D4E Startup RegKey

    ����: D4E Startup Regkey in c:\temp

    �^�N�e�B�N�X: �i����

    �d��x: ��

11. �u**����: ���[�� ���W�b�N��ݒ�@>**�v�{�^����I�����܂��B

12. �u���[�� ���W�b�N�̐ݒ�v �^�u�ŁA**���[�� �N�G��**�����ɓ��͂���Ă���͂��ł��B

13. �N�G���X�P�W���[�����O�̏ꍇ�A���̂悤�ɐݒ肵�܂��B

- ������x�N�G�������s����: 5 ��
- �Ōォ��̃f�[�^�����Ă��������F 1 ��

**��** �����f�[�^�ɑ΂��ĈӐ}�I�ɑ����̃C���V�f���g�𐶐����Ă��܂��B  ����ɂ��A���{�͂����̃A���[�g���g�p�ł���悤�ɂȂ�܂��B

14. �c��̃I�v�V�����͊���l�̂܂܂ɂ��܂��B  �u**����: �C���V�f���g�̐ݒ� >**�v��I�����܂��B

15. �C���V�f���g�ݒ�ɂ́A�ȉ���ݒ肵�܂� 

- �C���V�f���g�̐ݒ�F �L��
- �A���[�g �O���[�v�F ����

�u**����: �������� >**�v��I�����܂��B

16. ���������^�u�Ŏ��̂悤�ɐݒ肵�܂��B

- PostMessageTeams-OnAlert ��I�����܂��B
- �u**����: ���r���[**�v ���N���b�N���܂��B

17. �m�F����э쐬 �^�u�ŁA**�쐬** ��I�����܂��B

### �^�X�N 3: SecurityEvent �ɂ��U��2�̌��o

���̃^�X�N�ł́A�Z�L�����e�B�C�x���g�R�l�N�^�� Sysmon ���C���X�g�[������Ă���z�X�g�ōU��2�̌��o���쐬���܂��B

���̍U���ɂ��A�V�������[�U�[���쐬����A���̃��[�U�[�����[�J���Ǘ��҂ɒǉ�����܂��B
```Command
net user theusernametoadd /add
net user theusernametoadd ThePassword1!
net localgroup administrators theusernametoadd /add
```

1. Azure Sentinel ���j���[�� �S�� �Z�N�V������**���O**��I�����܂��B

2. �܂��A�f�[�^���ۑ�����Ă���ꏊ���m�F����K�v������܂��B�U�����s�����΂���Ȃ̂�  

    ���O�̎��Ԕ͈͂��ߋ� 24 ���Ԃɐݒ肵�܂��B

3. �ȉ��́@KQL�@�X�e�[�g�����g�����s���܂�:

```KQL
search "administrators"
```

4. ���ʂ͎��̕\�������܂��B
    �C�x���g
    SecurityEvent

5. �ŏ��̃f�[�^�\�[�X�� SecurityEvent �ł��B�����O���[�v�ւ̃����o�[�̒ǉ������ʂ��邽�߂� Windows ���g�p����C�x���g ID �𒲍�����Ƃ������܂����B  ���� EventID �� Event �́A���������T���Ă�����̂ł��B    

4732 - �Z�L�����e�B���L���ȃ��[�J���O���[�v�Ƀ����o�[���ǉ�����܂����B

���̃X�N���v�g�����s���Ă��܂��B

```KQL
SecurityEvent
| where EventID == "4732"
| where TargetAccount == "Builtin\\Administrators"

```

6. �s��W�J���āA���R�[�h�Ɋ֘A���邷�ׂĂ̗��\�����܂��B  �T���Ă��郆�[�U�[���͕\������܂���B  ���́A���[�U�[����ۑ��������ɁA�Z�L�����e�B���ʎq �iSID�j ���ۑ������Ƃ������Ƃł��B  ����KQL�́ASID���ƍ����āAAdministrators �O���[�v�ɒǉ����ꂽ TargetUserName �Ƀf�[�^����͂��悤�Ƃ��܂��B


```KQL
SecurityEvent
| where EventID == "4732"
| where TargetAccount == "Builtin\\Administrators"
| extend Acct = MemberSid, MachId = SourceComputerId 
| join kind=leftouter (
     SecurityEvent 
     | summarize count() by TargetSid, SourceComputerId, TargetUserName
     | project Acct1 = TargetSid, MachId1 = SourceComputerId, UserName1 = TargetUserName
) on $left.MachId == $right.MachId1, $left.Acct == $right.Acct1 

```
����͗ǂ����o���[���̂悤�Ɍ����܂��B  

   ![�X�N���[���V���b�g](../Media/SC200_sysmon_attack3.png)

**��:** ���{�Ŏg�p�����f�[�^�Z�b�g�����������߁A���� KQL �͊��҂���錋�ʂ�Ԃ��Ȃ��ꍇ������܂��B

7. �A���[�g�ɂ��Ăł��邾�������̃R���e�L�X�g��񋟂��邱�Ƃɂ��A�Z�L�����e�B�^�p�A�i���X�g���x�����邱�Ƃ��d�v�ł��B����ɂ́A�����O���t�Ŏg�p����G���e�B�e�B�̓��e���܂܂�܂��B  ���̃N�G�������s���܂��B


```KQL
SecurityEvent
| where EventID == "4732"
| where TargetAccount == "Builtin\\Administrators"
| extend Acct = MemberSid, MachId = SourceComputerId 
| join kind=leftouter (
     SecurityEvent 
     | summarize count() by TargetSid, SourceComputerId, TargetUserName
     | project Acct1 = TargetSid, MachId1 = SourceComputerId, UserName1 = TargetUserName
) on $left.MachId == $right.MachId1, $left.Acct == $right.Acct1 
| extend timestamp = TimeGenerated, HostCustomEntity = Computer, AccountCustomEntity = UserName1

```

8. �K�؂Ȍ��o���[�����ł����̂ŁA�N�G���̂��郍�O �E�B���h�E�ŁA�R�}���h �o�[�� **�u�V�����A���[�g ���[���v** ��I�����A**�uAzure Sentinel �A���[�g�̍쐬�v** ��I�����܂��B

9. ����ɂ��A�A�i���e�B�N�X���[���E�B�U�[�h���N�����܂��B  �S�ʃ^�u�Ɏ��̂悤�ɓ��͂��܂�

- ����: SecurityEvents Local Administrators User Add 
- ����: SecurityEvents Local Administrators User Add 
- �^�N�e�B�N�X: �����G�X�J���[�V����
- �d��x: ��

�u**����: ���[�� ���W�b�N��ݒ�@>**�v�{�^����I�����܂��B

10. ���[�����W�b�N�̐ݒ�^�u�ŁA���[���N�G���G���e�B�e�B�ƃ}�b�v�G���e�B�e�B�����ɓ��͂���Ă���K�v������܂��B

11. �N�G���X�P�W���[�����O�̏ꍇ�A���̂悤�ɐݒ肵�܂��B

- ������x�N�G�������s����: 5 ��
- �Ōォ��̃f�[�^�����Ă��������F 1 ��

**��** �����f�[�^�ɑ΂��ĈӐ}�I�ɑ����̃C���V�f���g�𐶐����Ă��܂��B  ����ɂ��A���{�͂����̃A���[�g���g�p�ł���悤�ɂȂ�܂��B

12. �c��̃I�v�V�����͊���l�̂܂܂ɂ��܂��B  �u**����: �C���V�f���g�̐ݒ� >**�v��I�����܂��B

13. �C���V�f���g�ݒ�ɂ́A�ȉ���ݒ肵�܂� 

- �C���V�f���g�̐ݒ�F �L��
- �A���[�g �O���[�v�F ����
- �u**����: �������� >�v��I�����܂��B**

14. ���������^�u�Ŏ��̂悤�ɐݒ肵�܂��B

- **PostMessageTeams-OnAlert** ��I�����܂��B
- �u**����: �m�F >**�v�{�^����I�����܂��B

15. ���r���[ �^�u�ŁA**�쐬**��I�����܂��B

## ���K 7 �ɐi�݂܂��B
