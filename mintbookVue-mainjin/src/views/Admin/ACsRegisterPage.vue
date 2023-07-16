<template>
    <div id="total_wrap">
        <div id="divide_wrap">
            <div id="nav_wrap">
                <admin-menu-page></admin-menu-page>
            </div>
            <div id="content_wrap">
                <p id="maintitle">1:1문의 상세</p>
                <button id="edit">수정</button>
                <table class="table">
                    <tbody>
                        <tr>
                            <th scope="row">1:1문의 번호</th>
                            <td colspan="3">12</td>
                        </tr>
                        <tr>
                            <th scope="row">아이디</th>
                            <td>user1</td>
                            <th scope="row">작성자</th>
                            <td>0223-02-23</td>
                        </tr>
                        <tr>
                            <th scope="row" >제목</th>
                            <td td colspan="3">방주</td>
                        </tr>
                        <tr>
                            <th scope="row"  style="height: 100px;">내용</th>
                            <td td colspan="3">13</td>
                        </tr>
                        <tr>
                            <th scope="row" style="height: 200px;">답변</th>
                            <td colspan="3">답변답변답변답변답변답변답변답변답변답변답변답변답변답변답변답변답변답변</td>
                        </tr>
                    </tbody>
                </table>
                답변등록하기
                <button id="dap">답변 등록</button>
                <div>
                    <textarea class="dap1"></textarea>
                </div>
                
                
            </div>
        </div>
    </div>
</template>

<script>
import { reactive } from 'vue';
import AdminMenuPage from '../../components/AdminMenuPage.vue';

export default {
    components:{
        AdminMenuPage
    },

    setup () {
        const state = reactive({
            num:["1","2","3","4","5","6","7","8","9","10"]
        })

        return {state}
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
        width: 1800px;
    }
    

    
</style>