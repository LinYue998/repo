<template>
    <div class="todolist">
        <el-card>
            <el-alert
            title="ToDoList"
            type="info"
            center
            :closable="false">
            </el-alert>
            <!-- 输入框 -->
            <el-input v-model="toDoList.value" placeholder="请添加ToDoList" clearable @change="getToDo"></el-input>
            <el-button type="info" @click="arrSort">待办事项排序</el-button>
            <el-divider></el-divider>
            <el-row>
                    <h2>今日待办事项:</h2>
            </el-row>
            <!-- 添加的待办事项 -->
            <el-row>
                <el-col>
                    <el-tag
                    v-for="(item,i) in toDoArr"
                    :key="i"
                    closable 
                    @close="handleClose(i)"
                    >
                        <el-checkbox v-model="item.tick" @change="tickState(item.tick,item)"></el-checkbox>
                        <span>{{item.date}}</span>
                        <span>------------------------------</span>
                        <span>{{item.value}}</span>
                    </el-tag>
                </el-col>
            </el-row>
            
            <!-- 今日完成事项 -->
            <el-divider></el-divider>
            <h2>今日完成事项:</h2>
            <el-row>
                <el-col>
                    <el-tag
                    v-for="(item, i) in okToDo"
                    :key="i"
                    closable 
                    @close="handleOkClose(i)"
                    >
                        <el-checkbox v-model="item.tick" @change="tickFalseState(item.tick,item)"></el-checkbox>
                        <span>{{item.date}}</span>
                        <span>------------------------------</span>
                        <span>{{item.value}}</span>
                    </el-tag>
                </el-col>
            </el-row>
        </el-card>
    </div>
</template>

<script>
export default {
    data() {
        return {
            toDoList: {
                value: '',
                date: '',
                tick: false,
            },
            // 将每一次的值,存储到数组中
            toDoArr: [],
            // 完成事项的数组
            okToDo: [],
        }
    },
    created() {
        //页面刷新时,将保存到本地的数据拿到
        // 这里取值的时候如果为空，返回null，null.push就报错了
        this.getArr();
        this.getArray();
    },
    methods: {
        // 在输入框失去焦点或用户按下回车时触发
        getToDo() {
            const date = this.getDisDate();
            // 将处理后的时间赋给toDoLits对象
            this.toDoList.date = date;
            console.log(this.toDoList);
            // 将每一次输入的值，传给数组,然后将自身恢复默认
            this.toDoArr.push(this.toDoList);
            console.log(this.toDoArr);
            this.toDoList = {
                value: '',
                date: '',
                tick: false,
            };
            // 将数组缓存到本地
            this.setArr();
        },
        // 将数组toDoArr保存到本地
        setArr() {
            const localStorage = window.localStorage;
            localStorage.setItem('toDoArr',JSON.stringify(this.toDoArr));
        },
        //将保存到本地的数据拿到
        getArr() {
            const localStorage = window.localStorage;
            this.toDoArr = JSON.parse(localStorage.getItem('toDoArr')) || [];
        }, 
        // 将获取的时间进行处理
        getDisDate() {
            const dt = new Date();

            const y = dt.getFullYear();
            const m = (dt.getMonth() + 1 + '').padStart(2, '0');
            const d = (dt.getDate() + '').padStart(2, '0');

            const hh = (dt.getHours() + '').padStart(2, '0');
            const mm = (dt.getMinutes() + '').padStart(2, '0');
            const ss = (dt.getSeconds() + '').padStart(2, '0');

            const currentDate = `${y}-${m}-${d} ${hh}:${mm}:${ss}`
            return currentDate;
        },
        handleClose(i) {
            this.toDoArr.splice(i,1);
            // 删除以后，将新数组保存到本地
            this.setArr();
            // 再重新获取一下数组
            this.getArr();
        },
        handleOkClose(i) {
            this.okToDo.splice(i,1);
            // 删除以后，将新数组保存到本地
            this.setArray();
            // 再重新获取一下数组
            this.getArray();
        },
        // 当未完成事项复选框值改变时，调用的函数
        tickState(i,item) {
            console.log(i);
            console.log(item);
            if(i == true){
                this.toDoArr.splice(item,1);
                this.setArr();
                this.getArr();

                this.okToDo.push(item);
                this.setArray();
            } return  
        },
        // 当完成事项复选框值改变时，调用的函数
        tickFalseState(i,item) {
            if(i == true){
                this.okToDo.splice(item,1);
                this.setArray();
                this.getArray();

                this.toDoArr.push(item);
                this.setArr();
            } return 
        },
        // 将数组toDoArr保存到本地
        setArray() {
            const localStorage = window.localStorage;
            localStorage.setItem('okToDo',JSON.stringify(this.okToDo));
        },
        //将保存到本地的数据拿到
        getArray() {
            const localStorage = window.localStorage;
            this.okToDo = JSON.parse(localStorage.getItem('okToDo')) || [];
        },
        // 待办事项排序
        arrSort() {
            // 先通过循环，将每一项的时间变为时间戳
            this.toDoArr.forEach(item => {
                const d = new Date(item.date);
                item.date = d.getTime();
                // console.log(item.date);
            });
            // 进行排序
            this.toDoArr.sort((a,b) => {
                return b.date-a.date;
            });
            // 再通过循环将它赋值回去
            this.toDoArr.forEach(item => {
                const tm = this.getTimeDate(item.date);
                item.date = tm;
            })   
            // console.log(this.toDoArr);
            this.setArr();
            this.getArr();
        },
        // 将获取的时间进行处理
        getTimeDate(td) {
            const dt = new Date(td);

            const y = dt.getFullYear();
            const m = (dt.getMonth() + 1 + '').padStart(2, '0');
            const d = (dt.getDate() + '').padStart(2, '0');

            const hh = (dt.getHours() + '').padStart(2, '0');
            const mm = (dt.getMinutes() + '').padStart(2, '0');
            const ss = (dt.getSeconds() + '').padStart(2, '0');

            const currentDate = `${y}-${m}-${d} ${hh}:${mm}:${ss}`
            return currentDate;
        },
    },
}
</script>

<style>
    .el-card{
        width: 650px;
        height: 650px;
        background: #ffffff;
        border: 1px solid #cecece;
        border-radius: 8px;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
    }
    .el-input{
        width: 50%;
        margin-top: 20px;
    }
    .el-tag{
        min-width: 50%;
        max-width: 100%;
        float: left;
        margin: 5px 5px;
        color: black;
    }
    h2{
        text-align: left;
        font-size: 22px;
        font-weight: bolder;
    }
    
</style>