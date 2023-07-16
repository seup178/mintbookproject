<template>
    <div id="total_wrap">
        <div id="divide_wrap">
            <div id="nav_wrap">
                <admin-menu-page></admin-menu-page>
            </div>
            <div id="content_wrap">
                <p id="maintitle">회원 상세</p>
                <button id="edit" @click="submit()">회원삭제</button>
                <p>회원정보</p>
                <table class="table">
                    <tbody>
                        <tr>
                            <th scope="row">아이디</th>
                            <td >{{state.member.member.email}}</td>
                            <th scope="row">이름</th>
                            <td >{{ state.member.member.name }}</td>
                        </tr>
                        <tr>
                            <th scope="row">주소</th>
                            <td colspan="3">{{ state.member.member.address }}</td>
                        </tr>
                        <tr>
                            <th scope="row">이메일</th>
                            <td >{{ state.member.member.email }}</td>
                            <th scope="row">휴대폰번호</th>
                            <td >{{ state.member.member.phone }}</td>
                        </tr>
                        <tr>
                            <th scope="row">성별</th>
                            <td >{{ state.member.member.gender }}</td>
                            <th scope="row">가입일</th>
                            <td >{{ state.member.member.joinDate }}</td>
                        </tr>
                        
                    </tbody>
                </table>
                <p>주문내역</p>
                <table class="table">
                    <thead>
                        <tr>
                            <th>날짜</th>
                            <th>주문번호</th>
                            <th>주문내역</th>
                            <th>주문번호</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(orders,tmp) in state.member.orders" :key="tmp">
                            <td>{{ orders.orderDate }}</td>
                            <td>{{ orders.orderNum }}</td>
                            <td>{{ orders.product }}</td>
                            <td>{{ orders.orderNum }}</td>
                        </tr>
                    </tbody>
                </table>
                <p>포인트 내역</p>
                <table class="table">
                    <thead>
                        <tr>
                            <th>충전/사용 날짜</th>
                            <th>금액</th>
                            <th>충전/사용</th>
                            <th>사용한 주문번호</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr>
                            <td> 2023-02-21</td>
                            <td>2000</td>
                            <td>사용</td>
                            <td>112594012349</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive } from 'vue';
import { useRoute } from 'vue-router';
import AdminMenuPage from '../../components/AdminMenuPage.vue';
import axios from 'axios';
import router from '@/router';

export default {
    components:{
        AdminMenuPage
    },

    setup () {
        const route = useRoute();

        const state = reactive({
            no  : Number( route.query.no ),
            member:"",
            email:String(route.query.email),
        })

        const load=() =>{
          axios.get(`/api/member/detail?no=${state.no}&email=${state.email}`).then((res)=>{
            console.log(res.data);
            state.member = res.data;
          })
        };
        load();

        const submit = () => {
            axios.delete(`/api/member/delete/${state.no}`).then((res)=>{
                console.log(res);
                window.alert("삭제성공");
                router.push({path: "/admin/member"});
            }).catch(()=>{
            window.alert("수정실패");
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
        margin-bottom:30px;
    }
    #edit{
        margin-left: 50px;
        float: left;
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
        margin-top:20px;
        margin-bottom: 30px;
        border: 1px solid black;
    }
    
    p{
        font-weight: bold;
        margin-bottom: 20px;
    }
    .table{
        width: 100%;
        margin-top:30px;
    }

    th{
        background: rgb(184, 184, 184);
    }

    #price{
        font-weight: bold;
        color: #3DDCA3;
    }

    
</style>