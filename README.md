using TMPro;
using Unity.VisualScripting;
using UnityEngine;

public class NewMonoBehaviourScript : MonoBehaviour
{
    Animator animator;
    bool Walk = false;

    public Vector2 inputVec;
    public float speed = 2f;
    public TextMeshProUGUI ComText;

    Rigidbody2D rigid;
    private bool isNearNPC = false;

     void Start()
     {
        Debug.Log("주인공 '지은'은 어릴 적부터 부모님의 권위 아래 자라왔습니다.");
        Debug.Log("지은이 20살이 되던해 '지은'의 부모님은 '빛의 집'이라는 종교 단체에 들어가게 되었고");
        Debug.Log("'지은'은 부모님을 따라 들어가게 되었습니다.");
        Debug.Log("처음에는 너무 좋고 평화로웠지만");
        Debug.Log("시간이 지날수록 '지은'은 이상한 낌새를 느끼게 됩니다.");
        Debug.Log("'지은'은 근처 폭포수 소리를 듣고 이 곳에서 폭포로 가면");
        Debug.Log("탈출할 수 있다는 믿음을 갖고 '지은'은 이 종교단체를 탈출하려고 계획을 세우게 됩니다...");
     }

    void Awake()
    {
        rigid = GetComponent<Rigidbody2D>();
        animator = GetComponent<Animator>();
    }

    
    void Update()
    {
        Walk = false;

        inputVec.x = Input.GetAxisRaw("Horizontal");
        inputVec.y = Input.GetAxisRaw("Vertical");

        if(inputVec.x > 0.5f || inputVec.x < -0.5f)
        {
            Walk =true;
        }
        else if (inputVec.y > 0.5f || inputVec.y < -0.5f)
        {
            Walk = true;
        }
        else
        {
            Walk = false;
        }
        animator.SetBool("Walk", Walk);

        if (isNearNPC && Input.GetKeyDown(KeyCode.E))
        {
            ComText.gameObject.SetActive(true);
            ComText.color = Color.white;
            ComText.text = "Hello";
        }
    }

    void FixedUpdate()
    {
        Vector2 nextVec = inputVec.normalized * speed * Time.fixedDeltaTime;
        rigid.MovePosition(rigid.position + nextVec);
    }

    private void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.gameObject.CompareTag("TLOAC"))
        {
            isNearNPC = true;
        }
    }

    private void OnTriggerExit2D(Collider2D collision)
    {
        if (collision.gameObject.CompareTag("TLOAC"))
        {
            isNearNPC = false;
            ComText.gameObject.SetActive(false);
        }
    }
}
