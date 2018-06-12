# -PSS-Foker-Hands-Problem
 - 2016년 1학기 성균관대학교 이진규 교수님의 문제해결기법 수업, 2번째 과제  
 - 한 카드를 바꿀 수 있는 Foker 게임의 승패 결정
 - 2016 1st semester Sungkyunkwan University Professor Jinkyu Lee's Problem Solving Strategies Class, 2st assignment
 - Determine the win or loss of a Foker game that can change one card
 
## 1. Problem
 - A Foker deck contains 32 cards. Each card has a suit of either clubs, diamonds, hearts, or spades (denoted C, D, H, S in the input data). Each card also has a value of either 2 through 9 (denoted 2,3,4,5,6,7,8,9). For scoring purposes card values are ordered as above, with 2 having the lowest and 9 the highest value. The suit has no impact on value.
 
 - A Foker had consists of three cards dealt from the deck. Foker hands are ranked by the following partial order from lowest to highest.  
 
    **High Card**. Hands which do not fit any higher categoryare ranked by the value of their highest card. If the highest cards have the same value, the hands are ranked by the next highest, and so on. (Suits do not affect the rank.)  
   
    **Pair**. Two of the three cards in the hand have the same value. Hands which both contain a pair are ranked by the value of the cards forming the pair. If these values are the same, the hands are ranked by the values of the cards not forming the pair, in decreasing order.  
   
    **Three of a Kind**. All the three cards in the hand have the same value. Hands which both contain three of a kind are ranked by the value of the three cards.  
   
    **Flush**. Hand contains three cards of the same suit. Hands which are both flushes are ranked using the rules for High Card.  
   
    **Straight**. Hand contains three cards with consecutive values. Hands which both contain a straight are ranked by their highest card.  
   
    **Straight Flush**. Three cards of the same suit with consecutive values. Ranked by the highest card in the hand.  
   
 - 한글 요약
   - 각 카드는 2~9의 숫자와 C, D, H ,S의 문양을 갖는다.
   
   - 두 명의 플레이어는 각각 3장의 카드를 받는다.
   
   - 3장의 카드의 조합은 주어진 족보에 의거하여 우선순위를 정하게 한다.
   
   - 각 플레이어는 자신의 보유한 카드 패의 우선순위를 높이기 위하여 3장의 카드 중에 한 장의 카드의 숫자를 임의로 바꿀 수 있다.

 - Your job is to compare several pairs of Foker hands and to indicate which, if either, has a higher rank. Each player has 3 cards, but each player can change the value of one card (to any value) so as to make the player's Foker hand as high as possible.
 
 - Input : The input file contains several lines, each containing the designation of 6 cards: the first three cards are the hand for the player named "Black" and the next three cards are the hand for the player named "White".
 - Output : For each line of input, print a line containing one of the following:
   - Tie.
   - Black wins.
   - White wins.
 - Sample Result
 
   ![image](https://user-images.githubusercontent.com/26705935/41288230-94e9166a-6e80-11e8-81b7-ad988945126e.png)
    
## 2. Environment
 - language : C
 - IDE : Microsoft Visual studio 2017, [ideone](https://ideone.com/)s 
 
## 3. Problem solving strategy
### (1) 가장 높은 우선순위의 족보부터
 - 카드 패의 우선순위를 결정하는 족보는 high card, pair, three of a kind, flush, straight, straight flush가 있다. 이들 중에서 가장 높은 우선순위를 결정하는 straight flush부터 거꾸로 생각해보기로 하였다. 또한 플레이어가 한 카드의 숫자를 바꿀 수 있다는 점을 크게 고려하였다.
 
 - 아이디어
 
   - 플레이어는 자신의 마음에 들지 않는 카드 한 장의 숫자를 임의로 바꿀 수 있다. 결국 우선순위를 결정하는 것은 세 카드 중에 두 카드이다. 가장 높은 족보부터 그 족보가 나타날 수 있는 경우를 모두 세어보았다. 한 카드의 숫자를 바꾸어 만든 우선순위가 가장 높은 패의 족보는 숫자로 나타내어, 나중에 승부를 겨룰 때에 보다 쉽게 비교할 수 있도록 한다. (족보 점수라고 하자.)
   
 1.	**Straight Flush와 Straight**
     - Straight Flush는 세 카드의 문양이 같으면서도, 숫자가 연속 되어야 한다. Straight는 문양에 상관 없어 숫자만 연속적이면 된다. 세 장의 카드가 straight가 되는 경우는 크게 두 가지이다. 세 장 중에 두 장이 연속적이거나, 세 장 중에 두 장의 차이가 2인 경우이다. 
     - 첫 번째의 경우에는 나머지 한 장의 카드를 두 장에 이어 연속한 수로 바꾸면 가능하다. 예를 들어, 8H 5H 4D의 패를 받은 경우, 5H와 4D가 연속적이므로 8H의 8을 나머지 두 수에 이어 연속한 수로 바꾼다면 (6 또는 3이 될 것이다.) Straight를 만들 수 있다. 연속한 수는 보통 두 가지로 나타나는데, 같은 Straight 내에서는 가장 큰 수가 큰 패가 유리하므로, 연속한 수 중 큰 수로 바꾼다.
     - 만일 두 장이 연속하지만 한 장의 카드의 숫자가 9인 경우, 나머지 한 장의 카드를 연속하면서도 큰 수로 잡을 수 없다. 예를 들어 9H 8D 3D라는 카드에서는 3을 9, 8과 연속한 수로 바꾸어 Straight를 만들 수 있는데, 9보다 큰 수로 바꾸지는 못하므로 이런 경우에는 3을 연속한 수 중 작은 수인 7로 바꾼다.
     - 두 번째의 경우에는 나머지 한 장의 카드를 두 장 사이의 수로 바꾸면 Straight가 가능하다. 예를 들어 8H 6D 2S의 패를 받은 경우, 2를 8과 6 사이의 숫자인 7로 바꾼다면 Straight를 만들 수 있다.
     - Straight Flush는 숫자를 바꾸어 만든 패가 Straight인 경우에 대해 추가적으로, 카드의 문양을 검사하여 알아낼 수 있다. 만일 Straight를 이루는 패의 카드의 문양이 모두 같다면, 이는 Straight Flush가 된다.
     - Straight Flush는 족보 점수 52~59점을 갖고, Straight는 족보 점수 42~49점을 갖는다. 일의 자리는 가장 높은 카드의 숫자를 의미한다.
     
 2.	**Flush**
    - Flush는 세 카드의 문양이 모두 같은 경우이다. 문양은 플레이어가 임의로 바꿀 수 없기 때문에, 받은 세 카드의 문양을 그대로 조사한다. 만일 세 카드가 같은 문양을 가지고 있다면, Flush가 된다. 만일 두 플레이어가 모두 Flush라면, 세 카드의 숫자 중에 가장 높은 숫자를 비교하여 더 높은 숫자를 가지고 있는 플레이어가 승리한다. 그러므로 한 장의 카드의 숫자를 바꾸는 기회는 아무 숫자를 9로 바꾸는 것으로 한다. 9 Flush는 Flush 족보 중에 가장 높은 우선순위를 갖기 때문이다.
    - Flush의 족보 점수는 32~39점을 갖고, 일의 자리는 가장 높은 카드의 숫자를 의미한다. 하지만 플레이어가 임의로 한 카드의 숫자를 바꾸는 기회가 있기 때문에 모든 Flush의 족보 점수는 39점이 될 것이다.
   
 3. **Three of a kind**
    - Three of a kind는 세 카드의 숫자가 같은 경우이다. 플레이어는 패를 받은 후에 한 장의 카드의 숫자를 교환할 기회가 있기 때문에, 만일 두 장의 카드의 숫자가 같다면, 나머지 한 장의 숫자를 바꾸어 Three of a kind 족보를 만들 수 있다.
    - Three of a kind의 족보 점수는 22~29점을 갖고, 일의 자리는 카드의 숫자를 의미한다.

 4. **Pair**
    - Pair는 세 장 중에 두 장의 카드의 숫자가 같은 경우이다. 한 장의 카드의 숫자를 바꾸는 기회를 고려한다면, 임의의 세 장의 카드를 받아도 pair 족보를 만들 수 있다. 그러므로 세 장의 카드 중에서 가장 큰 숫자를 갖는 카드로 맞추어 pair 족보를 만든다.
    - Pair의 족보 점수는 12~19점을 갖고, 일의 자리는 pair를 이루는 숫자를 의미한다. 만일 두 플레이어가 똑같은 숫자의 카드로 pair 족보를 가지고 있다면, 승부는 pair를 이루지 않는 나머지 한 장의 숫자에 따라 결정된다. 나머지 카드의 숫자가 클수록 족보의 우선순위가 높아진다. (in decreasing order) 이는 족보 점수에 반영하지는 않았지만, 승부를 결정할 때에 고려할 사항이다.

 - 이점
   - 족보를 거꾸로 생각하는 것은, 한 장의 카드의 숫자를 바꾸는 단계에서 가능한 우선순위가 높은 족보로 바꾼다는 것을 의미한다. 우선순위가 가장 높은 족보부터 생각하면, 후에 그보다 우선순위가 낮은 족보로 교환 가능해도 바꾸지 않는다. 또한 족보 점수라는 아이디어는 승부의 결과를 보다 쉽게 판단할 수 있고, 같은 족보 상에서 숫자가 높은 카드를 갖는 플레이어가 승리한다는 규칙을 보다 쉽게 반영할 수 있다.

## 4. Result
 - 이 프로그램이 정확한 결과값을 도출하는지를 알아보기 위해서 간단한 여섯 장의 카드를 예시로 만들었다.
 
   ![image](https://user-images.githubusercontent.com/26705935/41288806-8f794ea0-6e82-11e8-937a-e45c7c3a6f07.png)
   
 - 예시 1은 Black에게 4D 4H 6C의 카드를, White에게 6H 3D 2C의 카드를 준다. Black은 4와 6의 차이가 2이므로 이를 바탕으로 Straight를 만들 수 있다. (모양이 달라서 Straight Flush는 되지 않는다.) 바뀐 Black의 패는 5D 4H 6C가 되어 46점의 족보 점수를 얻을 것이다. White는 3과 2의 차이가 1이므로 이를 바탕으로 Straight를 만들 수 있다. (모양이 달라서 Straight Flush는 되지 않는다.) 바뀐 White의 패는 4H 3D 2C가 되어 44점의 족보 점수를 얻을 것이다. 이에 따라 Black이 승리할 것이고, 프로그램은 Figure 2와 같이 정확한 결과를 나타내었다.
 
   ![image](https://user-images.githubusercontent.com/26705935/41288853-b8300488-6e82-11e8-936e-8a9797a5c49b.png)
   
 - 예시 2는 Black에게 9D 5H 2C의 카드를, White에게 9H 6D 3S의 카드를 준다. 두 패는 모두 pair 이외의 다른 족보를 나타내지 못한다. 따라서 Black과 White 모두 19점의 족보 점수를 얻는다. 승부를 결정하는 과정에서 pair를 이루지 않은 나머지 한 장의 숫자의 크기를 비교한다. Black의 숫자는 5이고 White의 숫자는 6이므로, White의 숫자가 더 크기 때문에 White가 승리할 것이다. 프로그램은 Figure 4와 같이 정확한 결과를 나타내었다.
 
## 5. Conclusion
 - 본 문제는 두 명의 플레이어가 세 장의 카드를 받고, 족보를 결정하여 승부를 출력하는 프로그램이다. 높은 우선순위의 족보부터 생각하였다는 점과, 족보 점수라는 변수를 도입하는 것으로 본 프로그램을 구현하였다. 한 장의 카드의 숫자를 임의로 바꿀 수 있다는 점이 문제의 가장 큰 변수였지만, 세 장 중에 두 장만으로 결정할 수 있는 족보를 먼저 생각함으로써 나머지 한 장의 숫자를 모든 가능한 숫자로 보았다. 문제의 큰 변수가 오히려 힌트가 되었다고 생각한다.
