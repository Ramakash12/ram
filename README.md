Perfect bhai! Unity ke liye mai ek simple 2D fighting game ka base code structure de raha hoon — jisme basic features honge:

Player aur Enemy

Movement (left/right)

Punch attack

Health system

Win/lose condition


1. PlayerController.cs

using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float moveSpeed = 5f;
    public Animator animator;
    public int maxHealth = 100;
    private int currentHealth;

    void Start()
    {
        currentHealth = maxHealth;
    }

    void Update()
    {
        float move = Input.GetAxis("Horizontal");
        transform.Translate(Vector2.right * move * moveSpeed * Time.deltaTime);

        animator.SetFloat("Speed", Mathf.Abs(move));

        if (Input.GetKeyDown(KeyCode.Space))
        {
            animator.SetTrigger("Punch");
        }
    }

    public void TakeDamage(int damage)
    {
        currentHealth -= damage;
        if (currentHealth <= 0)
        {
            Die();
        }
    }

    void Die()
    {
        Debug.Log("Player Died");
        // Game Over logic
    }
}


---

2. EnemyController.cs

using UnityEngine;

public class EnemyController : MonoBehaviour
{
    public int maxHealth = 100;
    private int currentHealth;

    void Start()
    {
        currentHealth = maxHealth;
    }

    public void TakeDamage(int damage)
    {
        currentHealth -= damage;
        if (currentHealth <= 0)
        {
            Die();
        }
    }

    void Die()
    {
        Debug.Log("Enemy Defeated");
        Destroy(gameObject);
    }
}


---

3. PunchTrigger.cs (Attach to player's hand as a trigger)

using UnityEngine;

public class PunchTrigger : MonoBehaviour
{
    public int damage = 20;

    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Enemy"))
        {
            other.GetComponent<EnemyController>().TakeDamage(damage);
        }
    }
}


---

4. Unity Setup

2D Scene setup with:

Player and Enemy sprites

Rigidbody2D + BoxCollider2D

Animator with animations: Idle, Run, Punch


Add PlayerController to player object.

Add EnemyController to enemy.

Add a child object to player’s hand with PunchTrigger and IsTrigger enabled
using UnityEngine;

public class EnemyController : MonoBehaviour
{
    public int maxHealth = 100;
    private int currentHealth;

    void Start()
    {
        currentHealth = maxHealth;
    }

    public void TakeDamage(int damage)
    {
        currentHealth -= damage;
        if (currentHealth <= 0)
        {
            Die();
        }
    }

    void Die()
    {
        Debug.Log("Enemy Defeated");
        Destroy(gameObject);
    }
}

using UnityEngine;

public class PunchTrigger : MonoBehaviour
{
    public int damage = 20;

    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Enemy"))
        {
            other.GetComponent<EnemyController>().TakeDamage(damage);
        }
    }
}

