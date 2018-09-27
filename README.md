## Scene Setup:
- Create two 2D Objects name them “Missile ” and “Target”.
- Add a Sprite to them. (You can download the project from below link).
- Add rigidBody 2D in missile and add Script also name it as ”Homing Missile”


## How it actually works?
**You might think,**
It is easy to Implement Homing Missile. Just use MoveToward/Lerp until it reaches the target. Then just write code and look at what is going on in your scene. (try it and then read further to understand)

![](http://www.theappguruz.com/app/uploads/2018/06/working-of-homing-missile.png)

It is reaching the target properly but there is no feel of a rocket. (Right Brain:- **What is wrong with my code?**)

Checkout an actual script for Homing Missiles.

**Scripting**

```csharp
public class HomingMissile : MonoBehaviour
{
    public Transform target;
    public Rigidbody2D rigidBody;
    public float angleChangingSpeed;
    public float movementSpeed;
    void FixedUpdate ()
    {
        Vector2 direction = (Vector2)target.position - rb.position;
        direction.Normalize ();
        float rotateAmount = Vector3.Cross (direction, transform.up).z;
        rigidBody.angularVelocity = -angleChangingSpeed * rotateAmount;
        rigidBody.velocity = transform.up * movementSpeed;
    }
}
```
I think script is short but not a sweet one. You can use this homing missile script for your code.

Don’t be scared, I’ll show you how to do it.

## Pure Mathematics: (Open your Brain’s left part)

![](http://www.theappguruz.com/app/uploads/2018/06/actual-path-of-homing-missile.png)

You need to understand this Picture. That’s How our Homing Missile Works.

First, what you need is “Direction”.

## STEP: 1

```csharp
direction= targetPosition (target.position) - missilePosition(rb.position);
```
![](http://www.theappguruz.com/app/uploads/2018/06/vector-substraction.png)

This is how subtraction of two Vector happens (Pythagoras Theorem).

## STEP: 2
```csharp
direction.Normalize();
```
Normalization means converting vector to unit vector.

**Example:**
```csharp
Vector A(3,4)
Normalized(A)=(3/3*3+4*4 , 4/3*3+4*4)=(3/5,4/5)
```
## STEP: 3
Find Amount of rotation to do that.

```csharp
float rotateAmount=Vector3.Cross(direction,transform.up).z;
```
![](http://www.theappguruz.com/app/uploads/2018/06/vector-cross-product.gif)

**Blue Arrow(direction)**

**Red Arrow(Missile Transform)**

**green(Resulting Vector)**

All three Vectors are perpendicular to each other.that’s why we needed z-axis only.

## STEP: 4
Now we need to change angularVelocity of our rigidBody.
```csharp
rb.angularVelocity = - angleChangingSpeed * rotationAmount.
```
Minus Sign(-) is used to reverse the direction of movement.

## STEP: 5
Adding Velocity to our rigidBody.(just changing the Angle is not Enough).

```csharp
rb.velocity=tranform.up * movementSpeed.
```

**Stop cheering just yet!**

**This is not the end!**

Missile needs balancing between **angleChangingSpeed** and **movementSpeed**.

What if there is no balance between **angleChangingSpeed** and **movementSpeed** (Great question! Open right brain is needed here ! )

![](http://www.theappguruz.com/app/uploads/2018/06/missile-heading-to-the-target.png)

In the above Image , the missile is heading forward to the target, there is no problem here.

![](http://www.theappguruz.com/app/uploads/2018/06/velocity-force-vs-angular-velocity.png)

In the above Image, the target has moved to a new position. The missile continues to go toward the target due to angular velocity (in red) , while still going to the place where the target used to be (in black) due to its velocity. (Thinking what is wrong.?)

![](http://www.theappguruz.com/app/uploads/2018/06/velocity-force-vs-angular-velocity-2.png)

In the above Image, the missile's velocity forces it to move at the side of the target (black) while the angular velocity tries to pull the missile to the target. (Now you might feel there is something suspicious).

![](http://www.theappguruz.com/app/uploads/2018/06/orbit-dead-loop.png)

In the above Image, the missile falls into a stable orbit around the target and never reaches its goal.

The black arrows indicate a velocity vector while the red lines indicates angular velocity. (That’s why need a balance between **angleChangingSpeed** and velocity).

Considering that there is no friction in space, there is nothing to slow the velocity of the missile down and collapse the orbit.

That is how whole Universe is trapped (Thank God for this error else there is no life!).

**Now, How to Fix this Problem ?**

Well, There is no particular formula to balance them, but by Increasing angle or decreasing velocity will help you there.

## Caution
- Do not go with very small speed so your missile loses the feel of Homing Missile and Do not make it Extreme fast it will be trapped in the orbit.

## Caution
- Do not make **angleChangingSpeed** too small else it will fall in to trap and to high **angleChangingSpeed** loses the feel of your game.

Thats It for Homing Missile from my side..!
