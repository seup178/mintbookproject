<template>
    <div id="total_wrap">
      <div id="divide_wrap">
        <div id="nav_wrap">
          <admin-menu-page></admin-menu-page>
        </div>
        <div id="content_wrap">
          <p id="maintitle">도서 상세</p>
          <button id="edit" @click="goRevise()">수정</button>
          <table class="table">
            <tbody>
              <tr>
                <th scope="row">도서명</th>
                <td colspan="3">{{ state.oneBookData.bookName }}</td>
              </tr>
              <tr>
                <th scope="row">저자명</th>
                <td>{{ state.oneBookData.author }}</td>
                <th scope="row">출판사</th>
                <td>{{ state.oneBookData.publisher }}</td>
              </tr>
              <tr>
                <th scope="row">출판일</th>
                <td>{{ state.oneBookData.publishDate }}</td>
                <th scope="row">정가</th>
                <td>{{ state.oneBookData.price }}</td>
              </tr>
              <tr>
                <th scope="row">ISBN</th>
                <td>{{ state.oneBookData.isbn }}</td>
                <th scope="row">카테고리</th>
                <td>{{ state.oneBookData.genre }}</td>
              </tr>
              <tr>
                <th scope="row">판매수</th>
                <td>{{ state.oneBookData.hit }}</td>
                <th scope="row">재고수</th>
                <td>{{ state.oneBookData.hit }}</td>
              </tr>
              <tr>
                <th scope="row">작가소개</th>
                <td colspan="3">{{ state.oneBookData.authorInfo }}</td>
              </tr>
              <tr>
                <th scope="row">책소개</th>
                <td colspan="3">{{ state.oneBookData.content }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { onMounted, reactive } from "vue";
  import AdminMenuPage from "../../components/AdminMenuPage.vue";
  import axios from "axios";
  import { useRoute, useRouter } from "vue-router";
  
  export default {
    components: {
      AdminMenuPage,
    },
  
    setup() {
      const router = useRouter();
  
      const route = useRoute();
  
      const state = reactive({
        id: route.query.id,
        oneBookData: [],
      });
  
      //도서 1개 데이터 수신
      const oneBookData = async () => {
        await axios
          .get(`/api/bookone?id=${state.id}`)
          .then((res) => {
            console.log("도서 1개 데이터", res);
            state.oneBookData = res.data;
          })
          .catch((err) => {
            console.log(err);
          });
      };
  
      //수정페이지 이동
      const goRevise = () => {
        router.push({ path: "/admin/book/edit", query: { id: state.id } });
      };
  
      onMounted(() => {
        oneBookData();
      });
  
      return { state, goRevise };
    },
  };
  </script>
  import
  <style lang="css" scoped>
  /*초기화 */
  * {
    padding: 0;
    margin: 0;
  }
  
  ul,
  li {
    list-style: none;
  }
  
  a {
    text-decoration: none;
    color: black;
  }
  a:hover {
    color: black;
    font-weight: bold;
  }
  
  /*전체 페이지 영역 */
  #divide_wrap {
    /* width: 1920px; */
    display: grid;
    grid-template-columns: 290px auto;
  }
  
  #content_wrap {
    padding-left: 30px;
    padding-right: 30px;
  }
  
  /*사이드메뉴 영역 */
  #nav_wrap {
    background: rgb(223, 223, 223);
    height: 100vh;
  }
  
  /*콘텐츠 상단 영역 */
  #maintitle {
    font-size: 24px;
    font-weight: bold;
    margin-top: 50px;
    margin-bottom: 20px;
  }
  
  button {
    width: 100px;
    border-radius: 5px;
    color: white;
    background: black;
    margin-bottom: 30px;
  }
  
  #edit {
    margin-left: 50px;
    float: right;
  }
  
  /*콘텐츠 하단 영역 */
  
  .table {
    width: 100%;
    margin-top: 30px;
  }
  
  th {
    background: rgb(184, 184, 184);
  }
  </style>
  