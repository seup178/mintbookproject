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
          <li><a href="/mypage/mypage" id="current">구매내역</a></li>
          <li><a href="/mypage/mypick">찜</a></li>
          <li><a href="/mypage/cashpoint">캐시/포인트</a></li>
          <li><a href="/mypage/inquire">1:1 문의내역</a></li>
          <li><a href="/mypage/review/list">리뷰내역</a></li>
          <li><a href="/mypage/alimi">알리미</a></li>
          <li><a href="/mypage/myinfoedit">회원정보수정</a></li>
        </ul>
      </div>
    </div>
    <div id="right_content">
      <h4>구매내역</h4>
      <table class="table">
        <thead>
          <tr>
            <th scope="col">주문일</th>
            <th scope="col">주문번호</th>
            <th scope="col">주문금액</th>
            <th scope="col">주문도서</th>
            <th scope="col">상태</th>
            <th scope="col">비교</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item,idx) in state.order" @click="handleDetail(item.id,state.id,item.orderNum)" :key="idx">
            <td>{{ item.orderDate }}</td>
            <td>{{ item.orderNum }}</td>
            <td>{{ item.price }}</td>
            <td>{{ item.product }}</td>
            <td>{{ item.status }}</td>
            <td><button class="btn">배송조회</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

import { reactive } from 'vue';
import { useRoute, useRouter } from 'vue-router';

export default {
  setup() {
    const router    = useRouter();
    const route     =useRoute();
    const state = reactive({
      order:[],
      no  : Number(route.query.no),
      id  : ""
    })

    const logincheck=()=>{
      axios.post('/api/members/logincheck').then((res)=>{
        console.log(res);
        //성공적으로 로그인
        state.id = res.data.id;
        }).catch(()=>{
          router.push({path: "/login"});
        })
    }
    logincheck();
    
    const load=()=>{
      axios.get('/api/mypage/read').then((res)=>{
        console.log(res.data);
        state.order = res.data;
      })
    }
    load();

    
    const handleDetail = (no,id,num) => {
      router.push({path:'/mypage/mypage/detail', query:{no:no,id:id,num:num}})
    }
    return {state, handleDetail};
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
.table{
        width: 100%;
        margin-top:30px;
        margin-bottom: 30px;
    }

.btn{
  width: 114px;
  border: 1px solid #b8b6b6;
}
</style>
