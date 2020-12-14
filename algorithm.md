수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

participant	completion	return
[leo, kiki, eden]	[eden, kiki] => 	leo
[marina, josipa, nikola, vinko, filipa]	[josipa, filipa, marina, nikola] =>	vinko
[mislav, stanko, mislav, ana]	[stanko, ana, mislav]	 => mislav

```
function solution(participant, completion) {
    var answer = '';
    participant.sort();
    completion.sort();
    
    answer = participant.find((item,index) => item != completion[index])
    return answer;
}
```
PYTHON
전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

구조대 : 119
박준영 : 97 674 223
지영석 : 11 9552 4421
전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.
```
def solution(phone_book):
    phone_book = sorted(phone_book)
    
    for p1,p2 in zip(phone_book,phone_book[1:]):
        if p2.startswith(p1):
            return False
        else:
            return True

```