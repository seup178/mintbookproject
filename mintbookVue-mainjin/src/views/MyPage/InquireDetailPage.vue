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
      <div id="section">
          <p id="maintitle">1:1문의접수</p>
          <table>
            <tr>
              <td align="center">제목</td>
              <td>
                <input type="text" placeholder="제목을 입력해주세요."  v-model="state.qna.qnaTitle" readonly/>
              </td>
            </tr>
            <tr>
              <td align="center">내용</td>
              <td>
                <textarea
                  name=""
                  id=""
                  placeholder="문의하실 내용을 입력해주세요."                  
                  v-model="state.qna.content"
                  readonly
                ></textarea>
              </td>
            </tr>
            <tr>
              <td align="center">답변</td>
              <td>
                <textarea
                  name=""
                  id=""
                  placeholder="답변 준비중"
                  v-model="state.qna.reply"
                  readonly
                ></textarea>
              </td>
            </tr>
          </table>
          <div id="btn_wrap">
            <button @click="submit()"><a href="/cs">삭제</a></button>
          </div>
        </div>

    </div>
  </template>
  
  <script>
  import { reactive } from '@vue/runtime-core';
  import axios from 'axios';
  import { useRoute } from 'vue-router';
  import router from '@/router';
  export default {
    setup() {
      const route = useRoute();


        const state = reactive({
            no      : Number( route.query.no ),
            qna:"",
        });

        const load=() =>{
          axios.get(`/api/mypage/inquire/detail?no=${state.no}`).then((res)=>{
            console.log(res.data);
            state.qna = res.data;
          })
        };
        load();

        const submit = () => {
        axios.delete(`/api/qna/delete/${state.no}`).then((res)=>{
          console.log(res);
          window.alert("삭제성공");
          router.push({path: "/mypage/inquire"});
        }).catch(()=>{
        window.alert("수정실패");
        })
      }



      return {state,submit};
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
  
  #current {
    font-weight: bold;
  }
  


  #maintitle {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 50px;
  margin-left: 50px;
  }

 
  #section {
  font-weight: bold;
  padding-top: 50px;
}

td {
  width: 367px;
  vertical-align: top;
  padding-bottom: 20px;
  
}

input {
  width: 100%;
  height: 39px;
  border: 1px solid #c0c0c0;
  border-radius: 6px;
  padding-left: 20px;
  outline: none;
}

textarea {
  border: 1px solid #c0c0c0;
  border-radius: 10px;
  width: 833px;
  height: 206px;
  resize: none;
  outline: none;
  padding: 20px;
  box-sizing: border-box;
  margin-bottom: 20px;
}

#btn_wrap {
  width: 100%;
  text-align: center;
  margin-left: 450px;
}

button {
  width: 114px;
  height: 37px;
  border-radius: 10px;
  border: 0.5px solid #9b9b9b;
  margin-left: 10px;
  cursor: pointer;
}
button:hover {
  font-weight: bold;
}
  
  
  
  </style>
  