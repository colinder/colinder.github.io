# form 태그


​		

# form 태그의 고찰

1. form tag의 inline 요소 중 **'action'**과 **'onsubmit'**

   ```html
   <form action="/oh my god" onsubmit="return test()">
       <input type="submit" value="hahaha">
   </form>
   <script>
   	function test(){
           console.log('HHHI')
       }
   </script>
   ```

   - submit 형태의 input을 클릭하게 되면, submit이 동작하면서 form의 'onsubmit'이 실행

   - 이후 에 form 태그의 'action'이 실행

     즉, 두가지의 행동이 실행됩니다. 

     

     ​		

   비효율적이지만, 만약 위와 같은 코드에서 나중에 실행되는 action은 동작하지 않게 하고 싶다면.

   ```html
   <form action="/oh my god" onsubmit="return test()">
       <input type="submit" value="hahaha">
   </form>
   <script>
   	function test(){
           console.log('HHHI')
           return false;				//👈 이것을 넣어줘서 action에 false를 전달해 동작을 막음
       }
   </script>
   ```

   ​	

   


