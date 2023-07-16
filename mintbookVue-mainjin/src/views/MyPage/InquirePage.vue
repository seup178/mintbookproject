<template>
  <div id="mypage_wrap">
    <div id="left_bar">
      <div id="profile_area">
        <div id="profile_img">
          <img
            src="../../assets/MyPage/mypage_userprofile.png"
            alt="mypage_profile"
          />
        </div>
      </div>
      <div id="menu_area">
        <ul>
          <li><a href="/mypage/mypage">구매내역</a></li>
          <li><a href="/mypage/mypick">찜</a></li>
          <li><a href="/mypage/cashpoint">캐시/포인트</a></li>
          <li><a href="/mypage/inquire" id="current">1:1 문의내역</a></li>
          <li><a href="/mypage/review/list">리뷰내역</a></li>
          <li><a href="/mypage/alimi">알리미</a></li>
          <li><a href="/mypage/myinfoedit">회원정보수정</a></li>
        </ul>
      </div>
    </div>
    <div id="right_content">
      <h4>1:1문의 내역</h4>
      <button class="btn"><a href="/cs/register">1:1문의하기</a></button>

      <table class="table">
        <tbody>
          <tr v-for="(item,idx) in state.qna" :key="idx" style="width: 80%;">
          <td class="Q">Q.</td>
          <td @click="handleDetail(item.id)" style="width: 300px; cursor: pointer; "   >{{ item.qnaTitle }}</td>
          <td>{{ item.reg_date }}</td>
          <td>
            <button class="table_btn" @click="handleedit(item.id)">수정</button>
            <button class="table_btn" @click="handledelete(item.id)">삭제</button>
          </td>
        </tr>
        </tbody>
                        
      </table>
    </div>
  </div>
</template>

<script>
import {reactive} from "vue";
import axios from "axios";
import { useRoute, useRouter } from 'vue-router';
export default {
  setup() {

    const logincheck=()=>{
      axios.post('/api/members/logincheck').then((res)=>{
        console.log(res);
        //성공적으로 로그인
      }).catch(()=>{
        router.push({path: "/login"});
      })
    }
        logincheck();
    const router    = useRouter();
    const route     =useRoute();
    const state = reactive({
      qna:[],
      no  : Number(route.query.no)
    })
    
    const load=() =>{
      axios.get('/api/qna/read').then((res)=>{
        console.log(res.data);
        state.qna = res.data;
      })
    };
    load();

    const handleDetail = (no) => {
      router.push({path:'/mypage/inquire/detail', query:{no:no}})
    }

    const handleedit = (no) => {
      router.push({path:'/mypage/inquire/edit', query:{no:no}})
    }




    const handledelete = (no) => {
      axios.delete(`/api/qna/delete/${no}`).then((res)=>{
          console.log(res);
          window.alert("삭제성공");
          router.push({path: "/mypage/inquire"});
        }).catch(()=>{
        window.alert("삭제실패");
        })
      }
    


    return {state,handleDetail, handleedit, handledelete};
  },
};
</script>

<style lang="css" scoped>
a {
  text-decoration: none;
  color: inherit;
}

#mypage_wrap {
  margin: 0 auto;
  width: 1200px;
  overflow: hidden;
  margin-top: 60px;
  display: flex;
}
#left_bar {
  width: 200px;
}
#profile_area {
  width: 176px;
  height: 146px;
  background-color: #3ddca3;
  border-radius: 15px;
  position: relative;
}
#profile_img {
  width: 70px;
  height: 70px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
#menu_area {
  width: 176px;
  height: 281px;
  border-radius: 20px;
  border: 3px solid #3ddca3;
  margin-top: 20px;
}
#menu_area > ul {
  width: 150px;
  margin-top: 20px;
}
#menu_area > ul > li {
  padding: 4px 0;
  font-size: 17px;
}
#right_content {
  width: 1000px;
  
  
}
#current {
  font-weight: bold;
}


.btn {
  width: 125px;
  height: 37px;
  border-radius: 6px;
  font-size: 18px;
  cursor: pointer;
  color: #3ddca3;
  background-color: #fff;
  border: none;
  margin-top: 25px;
  border: 1px solid #3ddca3;
  margin-left: 875px;
}


.table{
        width: 100%;
        margin-top:30px;
}

.Q{
  color: rgba(60, 220, 162, 1);
  height: auto;
  
  font-size: 24px;
  align-self: auto;
  font-style: Bold;
  text-align: left;
  font-family: Inter;
  font-weight: 700;
  line-height: normal;
  font-stretch: normal;
  margin-right: 0;
  margin-bottom: 0;
  text-decoration: none;
}
.table_btn {
  width: 80px;
  height: 37px;
  border-radius: 6px;
  font-size: 18px;
  cursor: pointer;
  color: #939393;
  background-color: #fff;
  border: none;
  
  border: 1px solid #737373;
  
}

</style>
