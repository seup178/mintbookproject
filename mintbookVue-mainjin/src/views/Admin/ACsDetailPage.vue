<template>
    <div id="total_wrap">
        <div id="divide_wrap">
            <div id="nav_wrap">
                <admin-menu-page></admin-menu-page>
            </div>
            <div id="content_wrap">
                <p id="maintitle">1:1문의 상세</p>
                <table class="table">
                    <tbody>
                        <tr>
                            <th scope="row">1:1문의 번호</th>
                            <td colspan="3">{{state.qna.id}}</td>
                        </tr>
                        <tr>
                            <th scope="row">아이디</th>
                            <td>{{ state.qna.writer }}</td>
                            <th scope="row">수정날짜</th>
                            <td>{{reg_date}}</td>
                        </tr>
                        <tr>
                            <th scope="row" >제목</th>
                            <td td colspan="3">{{ state.qna.qnaTitle }}</td>
                        </tr>
                        <tr>
                            <th scope="row"  style="height: 100px;">내용</th>
                            <td td colspan="3">{{ state.qna.content }}</td>
                        </tr>
                        <tr>
                            <th scope="row" style="height: 200px;">답변</th>
                            <td colspan="3">{{ state.qna.reply }}</td>
                        </tr>
                    </tbody>
                </table>
                <!--<button id="dap" @click="handeledit()">답변 등록</button>
                <button id="dap">답변 수정</button> -->
                답변 작성/수정하기
                <button id="dap"  @click="submit()">확인</button>
                <div>
                    <textarea class="dap1" v-model="state.qnadto.reply"></textarea>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive } from 'vue';
import AdminMenuPage from '../../components/AdminMenuPage.vue';
import axios from 'axios';
import { useRoute } from 'vue-router';
import router from '@/router';


export default {
    components:{
        AdminMenuPage
    },

    setup () {
        const route = useRoute();
        
        const state = reactive({
            no      : Number( route.query.no ),
            qna:"",
            qnadto:{
                reply:""
            }
        });

        

        const load=() =>{
          axios.get(`/api/mypage/inquire/detailadmin?no=${state.no}`).then((res)=>{
            console.log(res.data);
            state.qna = res.data;
          })
        };
        load();
       
        const submit = () => {
        axios.post(`/api/qna/reply/${state.no}`,state.qnadto).then((res)=>{
          console.log(res);
          window.alert("답변완료");
          router.push({path: "/admin/cs"});
        }).catch(()=>{
        window.alert("답변실패");
        })
      }





        return {state,submit}
    }
}
</script>
import 
<style lang="css" scoped>
    /*초기화 */
    *{
    padding:0;
    margin:0;
    }

    ul,li{
        list-style: none;
    }

    a{
        text-decoration: none;
        color: black;
    }
    a:hover{
        color: black;
        font-weight: bold;
    }

    /*전체 페이지 영역 */
    #divide_wrap{
        /* width: 1920px; */
        display: grid;
        grid-template-columns: 290px auto;
    }

    #content_wrap{
        padding-left: 30px;
        padding-right: 30px;
    }

    /*사이드메뉴 영역 */
    #nav_wrap{
        background: rgb(223, 223, 223);
        height: 100vh;
    }

    /*콘텐츠 상단 영역 */
    #maintitle{
        font-size: 24px;
        font-weight: bold;
        margin-top: 50px;
        margin-bottom:20px;
    }

    button{
        width: 100px;
        border-radius: 5px;
        color: white;
        background: black;
        margin-bottom:30px;
    }

    #edit{
        margin-left: 50px;
        float: right;
    }

    /*콘텐츠 하단 영역 */

    .table{
        width: 100%;
        margin-top:30px;
    }

    th{
        background: rgb(184, 184, 184);
    }


    #dap{
        margin-left: 50px;
        float: right;
    }
    .dap1{
        width: 1200px;
        height: 300px;
    }
    

    
</style>