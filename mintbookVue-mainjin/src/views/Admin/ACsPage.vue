<template>
    <div id="total_wrap">
        <div id="divide_wrap">
            <div id="nav_wrap">
                <admin-menu-page></admin-menu-page>
            </div>
            <div id="content_wrap">
                <p id="maintitle">1:1문의 관리</p>
                <label>
                    <select>
                        <option value="title">회원관리</option>
                    </select>
                </label>
                <label>
                    <input type="text" v-model="state.search">
                </label>
                <button id="search" @click="UserSearch()">검색</button>
                <button id="register">도서등록</button>
                <table class="table">
                    <thead>
                        <tr>
                            <th scope="col">ID.</th>
                            <th scope="col">제목</th>
                            <th scope="col">작성한 회원 아이디</th>
                            <th scope="col">작성 날짜</th>
                            <th scope="col">답변완료</th>     
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item,idx) in state.qna" :key="idx" @click="handleDetail(item.id)" style="cursor: pointer;">
                            <th scope="row">{{ (idx+1 )+((board.page-1)*10) }}</th>
                            <td>{{ item.qnaTitle }}</td>
                            <td>{{ item.writer }}</td>
                            <td>{{ item.reg_date }}</td>
                            <td v-if="item.reply != null">답변 완료</td>
                            <td v-else >답변 대기중</td>
                            
                        </tr>
                    </tbody>
                </table>
                <div id="pagination">
                    <el-pagination @current-change="handlePage" small layout="prev, pager, next"  :total="board.total"/>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive } from 'vue';
import AdminMenuPage from '../../components/AdminMenuPage.vue';
//import router from '@/router';
import axios from 'axios';
import { useRoute, useRouter } from 'vue-router';

export default {
    components:{
        AdminMenuPage
    },

    setup () {
        const router    = useRouter();
        const route     =useRoute();
        const state = reactive({
            qna:[],
            no  : Number(route.query.no),
            search:""
        });
        const board = reactive({
            total : 0, //전체 게시물 수
            rows : null, //게시물 내용
            page : 1, //페이지정보
            text : '', //검색어 정보
        });

        const handlePage = (page) =>{     
            board.page= page; //상태변수값 변경
            axios.get(`/api/qna/all?page=${board.page}`).then((res)=>{
                console.log(res.data);
                state.qna = res.data;
            })            
        }

        const UserSearch=()=>{
            axios.get(`/api/qna/usersearch?search=${state.search}`).then((res)=>{
               console.log(res.data);
              state.qna = res.data;
              board.total = 0;
            })  
        }

        const load=() =>{
            //axios.get('/api/qna/all').then((res)=>{
            //    console.log(res.data);
            //   state.qna = res.data;
            //})     
            axios.get('/api/qna/allcnt').then((res)=>{
                console.log(res.data);
                board.total = res.data;
            })       
            handlePage(1);
        };
        load();
        const handleDetail = (no) => {
            router.push({path:'/admin/cs/detail', query:{no:no}})
        }
        

        return {
            state,
            board,
            //BookSearch,
            handlePage,
            handleDetail,
            UserSearch
        }
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

    /*콘텐츠 상단 영역 */
    #maintitle{
        font-size: 24px;
        font-weight: bold;
        margin-top: 50px;
        margin-bottom:70px;
    }

    button{
        width: 100px;
        border-radius: 5px;
        color: white;
        background: black;
    }

    select{
        width: 100px;
        height: 28px;
        margin-right: 20px;
    }

    input{
        margin-right: 10px;
    }

    #register{
        margin-left: 50px;
        float:right;
    }

    /*콘텐츠 하단 영역 */
    #nav_wrap{
        background: rgb(223, 223, 223);
        height: 100vh;
    }

    .table{
        width: 100%;
        margin-top:30px;
        margin-bottom: 30px;
    }

    #pagination{
        display: flex;
        justify-content: center;
    }
    

    
</style>