# <p align="center">꿈의 왕국 : 영원한 보금자리</p>

<p align="center">
<img src="https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/01355be0-c9d0-4f0e-a9bc-31ad52dac96c" width="200">
</p>

## 🎮게임 소개
장애물을 피해 하트를 모으며 10개의 스테이지를 통과하는 게임 &nbsp;

- 장르: 수집형 캐주얼 시뮬레이션 게임
- 특징: 폐허가 된 5개의 튜토리얼 섬과 3개의 메인 섬으로 구성된 꿈의 왕국을 되살리는 게임

&nbsp;

## 👩🏻‍💻개발 기간 & 개발 인원
- 개발 기간: 2023.03.23~2023.06.26(약 3개월)
- 개발 인원: 5인(프로그래머 5)
  
&nbsp;

## 🔗구글 플레이스토어 링크
https://play.google.com/store/apps/details?id=com.notsame.kingdomofdreams

&nbsp;

## ▶️플레이 영상
https://youtu.be/C9GL3ZHPBqE?si=HJIeB4Sw_RqOLXZG

&nbsp;

## ✏️팀 내 맡은 역할
- 팀장
- 타이틀 씬 제작
- 캐릭터 선택 씬 제작
- 오프닝 제작
- 클리어 씬 제작
- 플레이어 행동 제작(채광)
- 광산 맵 제작
- 꿈 광산 맵 제작
- 자체 BGM 제작(스테이지 6~8)
  
&nbsp;

## ❗타이틀
   > - 시네머신을 사용해 타이틀 화면 제작    
   
  ![타이틀](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/7d061e61-343a-442d-b7aa-ac8e20b49187)

&nbsp;

## ❗캐릭터 선택 
   > - 이진 트리를 사용해 질문의 대답에 따라 캐릭터가 달라지는 것을 구현

  ![캐릭터생성](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/f53392c1-fcc2-4500-b13a-1512b376bb1b)

<details>
 <summary>코드 보기</summary>
 
```ts
using UnityEngine;
using UnityEngine.UI;

public class UICreateCharacter : MonoBehaviour
{

    public Text questionText; // 질문을 보여주는 UI Text
    public GameObject[] answerImage;
    public GameObject[] objects;
    private GameObject currentObj;  //현재 오브젝트
    public Button answerButton1; // 대답 버튼 1
    public Button answerButton2; // 대답 버튼 2

    private TreeNode currentNode; // 현재 노드

    public GameObject resultPanel;
    public GameObject[] characters;
    public GameObject nicknamePanel;

    // 이진 트리 노드 클래스
    private class TreeNode
    {
        public GameObject key; // 키 (질문 또는 캐릭터 종류)
        public string value; // 값 (질문 또는 null)
        public int characterNum;
        public TreeNode left; // 왼쪽 자식 노드
        public TreeNode right; // 오른쪽 자식 노드

    }

    void Start()
    {
        this.CreateTree();

        answerButton1.onClick.AddListener(() =>
        {
            OnClickAnswerButton1();
        });

        answerButton2.onClick.AddListener(() =>
        {
            OnClickAnswerButton2();
        });
    }

    private void Update()
    {
        if (this.resultPanel.activeSelf)
        {
            this.nicknamePopup();
        }
    }


    public void CreateTree()
    {
        // 이진 트리 구성
        TreeNode root = new TreeNode { key = null, value = "선택하세요", characterNum = -1 };
        TreeNode node1 = new TreeNode { key = objects[0], value = "선택하세요", characterNum = -1 };
        TreeNode node2 = new TreeNode { key = objects[1], value = "선택하세요", characterNum = -1 };
        TreeNode node3 = new TreeNode { key = objects[2], value = "선택하세요", characterNum = -1 };
        TreeNode node4 = new TreeNode { key = objects[3], value = "선택하세요", characterNum = -1 };
        TreeNode node5 = new TreeNode { key = objects[2], value = "선택하세요", characterNum = -1 };
        TreeNode node6 = new TreeNode { key = objects[3], value = "선택하세요", characterNum = -1 };
        TreeNode node7 = new TreeNode { key = objects[4], value = null, characterNum = 00 };
        TreeNode node8 = new TreeNode { key = objects[5], value = null, characterNum = 01 };
        TreeNode node9 = new TreeNode { key = objects[4], value = null, characterNum = 02 };
        TreeNode node10 = new TreeNode { key = objects[5], value = null, characterNum = 03 };
        TreeNode node11 = new TreeNode { key = objects[4], value = null, characterNum = 04 };
        TreeNode node12 = new TreeNode { key = objects[5], value = null, characterNum = 05 };
        TreeNode node13 = new TreeNode { key = objects[4], value = null, characterNum = 06 };
        TreeNode node14 = new TreeNode { key = objects[5], value = null, characterNum = 07 };

        root.left = node1;
        root.right = node2;
        node1.left = node3;
        node1.right = node4;
        node2.left = node5;
        node2.right = node6;
        node3.left = node7;
        node3.right = node8;
        node4.left = node9;
        node4.right = node10;
        node5.left = node11;
        node5.right = node12;
        node6.left = node13;
        node6.right = node14;

        currentNode = root; // 현재 노드를 루트 노드로 초기화


        // 초기 질문 텍스트 설정
        questionText.text = currentNode.value;
    }

    void OnClickAnswerButton1()
    {
        if (currentNode.left != null)
        {
            currentNode = currentNode.left;
            Debug.LogFormat("<color=yellow>{0}</color>", currentNode.key);
            UpdateQuestionText(); //질문 텍스트 업데이트
            UpdateAnswerImage(); // 대답 이미지 업데이트

            if (currentNode.value == null)
            {
                Debug.Log("캐릭터 종류: " + currentNode.characterNum);

                this.resultPanel.SetActive(true);
                this.characters[currentNode.characterNum].SetActive(true);

                Debug.Log(currentNode.characterNum);
                InfoManager.instance.PlayerInfo.nowCharacterId = currentNode.characterNum;
                InfoManager.instance.PlayerInfo.myCharacters[currentNode.characterNum] = 1;
                InfoManager.instance.SavePlayerInfo();
            }
        }

    }

    void OnClickAnswerButton2()
    {
        if (currentNode.right != null)
        {
            currentNode = currentNode.right;
            Debug.LogFormat("<color=cyan>{0}</color>", currentNode.key);
            UpdateQuestionText(); //질문 텍스트 업데이트
            UpdateAnswerImage(); // 대답 텍스트 업데이트

            if (currentNode.value == null)
            {
                Debug.Log("캐릭터 번호: " + currentNode.characterNum);

                this.resultPanel.SetActive(true);
                this.characters[currentNode.characterNum].SetActive(true);

                Debug.Log(currentNode.characterNum);
                InfoManager.instance.PlayerInfo.nowCharacterId = currentNode.characterNum;
                InfoManager.instance.PlayerInfo.myCharacters[currentNode.characterNum] = 1;
                InfoManager.instance.SavePlayerInfo();
            }
        }
    }

    // 질문 텍스트 업데이트 함수
    void UpdateQuestionText()
    {
        if (currentNode.value != null)
        {
            questionText.text = currentNode.value;
        }
    }

    // 대답 텍스트 업데이트 함수
    void UpdateAnswerImage()
    {
        if (currentNode.left != null && currentNode.right != null)
        {
            answerImage[0].SetActive(true);
            answerImage[0].GetComponent<Image>().sprite = currentNode.left.key.GetComponent<Image>().sprite;

            answerImage[1].SetActive(true);
            answerImage[1].GetComponent<Image>().sprite = currentNode.right.key.GetComponent<Image>().sprite;
        }
        else if (currentNode.left == null && currentNode.right != null)
        {
            answerImage[0].SetActive(false);

            answerImage[1].SetActive(true);
            answerImage[1].GetComponent<Image>().sprite = currentNode.right.key.GetComponent<Image>().sprite;
        }
        else if (currentNode.left != null && currentNode.right == null)
        {
            answerImage[0].SetActive(true);
            answerImage[0].GetComponent<Image>().sprite = currentNode.left.key.GetComponent<Image>().sprite;

            answerImage[1].SetActive(false);
        }
    }

    public void nicknamePopup()
    {
        if (Input.GetMouseButtonDown(0))
        {
            this.characters[currentNode.characterNum].SetActive(false);

            EventManager.instance.onTouched();
        }
    }
}
```
▲ UICreateCharacter 스크립트
</details>
&nbsp;

## ❗오프닝
   > - 신규 유저일 경우 오프닝 동영상 재생
   > - 시네머신을 사용해 제작

https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/b8c145eb-f374-4b04-9624-0f54ff10e9a1

&nbsp;

## ❗클리어 씬
   > - 미션 완료 후 npc 부활 및 스테이지 부활 장면 제작
   > - 시네머신을 사용해 제작
   > - npc 부활의 경우 쉐이더를 이용해 흑백과 컬러를 Lerp 시킴

![npc01Clear-ezgif com-video-to-gif-converter](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/384c9c5a-1fe0-4a75-9299-9c887970c68c)  
![npc02Clear-ezgif com-video-to-gif-converter](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/fed67eda-2650-41ca-888f-a65166cca020)
![npc03Clear-ezgif com-video-to-gif-converter](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/c660741a-6ba7-4877-9545-acbc1f0b3150)
![npc04Clear-ezgif com-video-to-gif-converter](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/fc60adef-e8a4-4777-b80e-b4e187f893cd)
![npc05Clear-ezgif com-video-to-gif-converter](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/3e3485d7-7475-4714-819b-b53690f88ee0)  

▲ npc 부활 시네머신

&nbsp;

![01-ezgif com-video-to-gif-converter](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/59d11946-e3e8-417a-a77a-996850b3951d)
![0701-ezgif com-video-to-gif-converter](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/0b23089f-1b02-45f5-8fe6-526ad5d202b4)

▲ 스테이지 부활 시네머신 중 일부

&nbsp;

![엔딩](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/869b4d7e-1cdd-4e23-b2b5-bcef8269a934)

▲ 엔딩 시네머신

&nbsp;

## ❗채광
   > - 5가지의 행동 중 채광
   > - 광산에 들어가면 채광 가능
   > - 확률적으로 꿈 광산 입장 가능

![채광](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/3261bef7-d5de-459c-b4f3-264da87c3ca1)

▲ 광산

![꿈광산](https://github.com/gusdk337/KingdomOfDreamsResult/assets/51481890/be86d0dd-a7e4-44a2-a7f3-c0e201f3ce15)

▲ 꿈 광산


<details>
 <summary>코드 보기</summary>
 
```ts
using Cinemachine;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class MineMain : MonoBehaviour
{
    [SerializeField]
    private UIStage06Director director;
    public GameObject playerPrefab;
    public CinemachineVirtualCamera followCam;

    public int stageID;

    private Player player;

    private bool isClicked;

    private void Awake()
    {
        var go = GameObject.Find("UIStage06Director");
        go.SetActive(true);
        this.director = go.GetComponent<UIStage06Director>();
    }

    public void Start()
    {
        var mine_SC = SoundManager.GetSoundConnectionForThisLevel("Mine");
        SoundManager.PlayConnection(mine_SC);

        this.Init();
    }
    public void Init()
    {
        //플레이어, UI 생성
        this.player = new Player(this.playerPrefab);
        this.player.State = new NormalState(this.player);

        this.player.mono.Init();

        this.player.mono.joystick = this.director.joystick;
        this.followCam.Follow = this.player.mono.transform;
        this.followCam.LookAt = this.player.mono.transform;

        //상호작용 버튼 클릭
        this.director.btn_Interaction.onClick.AddListener(() => {
            this.isClicked = true;
            switch (this.player.mono.location)
            {
                case Enums.ePlayerLocation.Mine:
                    this.player.State = new MiningState(this.player);
                    break;
            }
            Debug.LogFormat("IState:{0}", this.player.State);
            this.player.DoAction();

            Invoke("IsClicked", 2f);
        });

        //미션 클리어 이벤트
        EventManager.instance.onAchieved = (data) => {

        };
    }
    private void Update()
    {
        //상호작용 버튼 활성화, 비활성화
        if (this.player.mono.isTargeting && this.player.mono.actionTarget != null && this.isClicked == false)
        {
            this.director.btn_Interaction.interactable = true;
            var atlas = AtlasManager.instance.GetAtlasByName("Interaction");
            var sprite = atlas.GetSprite("Icon_ItemIcon_Pickax");
            this.director.icon_Interaction.sprite = sprite;
            this.director.icon_Interaction.gameObject.SetActive(true);
        }
        else
        {
            this.director.btn_Interaction.interactable = false;
            this.director.icon_Interaction.gameObject.SetActive(false);
        }
    }

    public void IsClicked()
    {
        this.isClicked = false;
    }
}

```
▲ MineMain 스크립트

```ts
    private void Start()
    {
        lastIronPosition = transform.position;
    }

    public void Iron()
    {
        lastIronPosition = transform.position; // 철이 캐진 위치 기억
        gameObject.SetActive(false); // 철 비활성화
        GenerateIronPiece(); // 철 조각 생성
        GenerateDreamPieces(); //꿈 조각 생성
        Invoke("GenerateIron", ironRespawnTime); // 일정 시간 후 철 생성
    }

    private void GenerateIronPiece()
    {
        ironPiece = Instantiate(ironPiecePrefab, transform.position, transform.rotation);
        ironPiece.transform.position = lastIronPosition; // 철이 사라진 위치에 철 조각 생성
    }

```
▲ MiningIron 스크립트 중 일부

```ts
    public GameObject prefab; // 생성할 프리팹

    private List<Vector3> positions = new List<Vector3>(); // 생성한 위치를 저장할 리스트

    private void SpawnPrefabs()
    {
        for (int i = 0; i < 50; i++)
        {
            Vector3 spawnPosition = GetSpawnPosition();
            Quaternion spawnRotation = Quaternion.Euler(0f, Random.Range(0f, 360f), 0f);
            GameObject newPrefab = Instantiate(prefab, spawnPosition, spawnRotation);
            positions.Add(spawnPosition);
        }
    }

    private Vector3 GetSpawnPosition()
    {
        Vector3 spawnPosition = Vector3.zero;
        bool isOverlap = true;

        // 중복되지 않는 위치 찾기
        while (isOverlap)
        {
            float xPos = Random.Range(-10.85f, 9.73f);
            float zPos = Random.Range(5.07f, -5.79f);
            spawnPosition = new Vector3(xPos, 0f, zPos);

            isOverlap = false;
            foreach (Vector3 pos in positions)
            {
                if (Vector3.Distance(spawnPosition, pos) < prefab.transform.localScale.x)
                {
                    isOverlap = true;
                    break;
                }
            }
        }
        return spawnPosition;
    }

```
▲ DreamMineMain 스크립트 중 일부

```ts
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class MiningDream : MonoBehaviour
{
    public GameObject dreamPiecePrefab;

    private GameObject dreamPiece;
    private Vector3 lastIronPosition;

    private void Start()
    {
        lastIronPosition = transform.position;
    }


    public void Dream()
    {
        lastIronPosition = transform.position; // 꿈이 캐진 위치 기억
        gameObject.SetActive(false); // 꿈 비활성화
        GenerateDreamPiece(); // 꿈 조각 생성
    }

    private void GenerateDreamPiece()
    {
        dreamPiece = Instantiate(dreamPiecePrefab, transform.position, transform.rotation);
        dreamPiece.transform.position = lastIronPosition; // 꿈이 사라진 위치에 꿈 조각 생성
    }

}

```
▲ MiningDream 스크립트

```ts
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using TMPro;

public class UIDreamMineDirector : MonoBehaviour
{
    private int dreamCount;
    //페이드 아웃
    public Image imgPopup;
    public TMP_Text txtWelcome;

    public float fadeSpeed = 0.5f;
    public float currentAlpha = 1f;

    //슬라이더
    public Slider slider;
    public float maxTime = 30f;
    public float currentTime;
    public GameObject timeSlider;

    //결과창
    public Image imgResult;
    public TMP_Text txtCnt;

    public void Init()
    {
        currentTime = 0f;
        slider.minValue = 0f;
        slider.maxValue = maxTime;
        slider.value = currentTime;
        dreamCount = 0;

        EventManager.instance.dreamCount = (cnt) =>
        {
            this.dreamCount++;
        };
    }

    private void Update()
    {
        //팝업창 페이드 아웃
        this.FadeOutImage();

        //60초 타이머
        this.Timer();
    }

    public void FadeOutImage()
    {
        this.currentAlpha -= fadeSpeed * Time.deltaTime;

        if (this.currentAlpha <= 0)
        {
            this.imgPopup.gameObject.SetActive(false);
        }

        //알파 값 변경
        Color newColor0 = imgPopup.color;
        newColor0.a = currentAlpha;
        imgPopup.color = newColor0;

        Color newColor1 = txtWelcome.color;
        newColor1.a = currentAlpha;
        txtWelcome.color = newColor1;
    }

    public void Timer()
    {
        if(currentTime < maxTime)
        {
            currentTime += Time.deltaTime;
            slider.value = currentTime;
        }
        else if(currentTime >= maxTime)
        {
            currentTime = 0f;

            this.PlusDream(this.dreamCount);
        }
    }

    public void PlusDream(int cnt)
    {
        txtCnt.text = string.Format("X {0}", cnt);
        this.imgResult.gameObject.SetActive(true);


        InfoManager.instance.DreamAcount(cnt);

        StartCoroutine(WaitPlusDream());
    }
    private IEnumerator WaitPlusDream()
    {
        yield return YieldCache.WaitForSeconds(2f);

        EventDispatcher.instance.SendEvent<Enums.ePortalType>((int)LHMEventType.eEventType.ENTER_PORTAL, Enums.ePortalType.Stage);
    }
}

```
▲ UIDreamMineDirector 스크립트


</details>
