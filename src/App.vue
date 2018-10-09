/**
* @Description:    通知公告人员选择
* @Author:         TSY
* @CreateDate:     2018/9/5 9:04
* @email:          t@tsy6.com
*/
<template>
    <div>
        <div class="wrapper">
            <ul class="item_wrapper">
                <li class="item_first" v-for="(item,index) in jobList" :key="item.id">
                    <div @click="checkItem(index)" class="item">
                        <div class="checkGroup" @click="checkAll(index)" @click.stop>
                            <i class="iconfont"
                               :class="{'icon-gouxuan':isSelectAll[index] && isSelectAll[index].group,'icon-checkboxround0':isSelectAll[index] && !isSelectAll[index].group}"></i>
                        </div>
                        {{item.name}}
                    </div>
                    <ul class="item_second" v-show="isOpenItem[index] && isOpenItem[index].group">
                        <div class="item_third" v-for="(second,j) in item.son" :key="second.id">
                            <div @click="checkSecondItem(index, j)" class="item">
                                <div class="checkGroup" @click="checkSecondAll(index, j)" @click.stop>
                                    <i class="iconfont"
                                       :class="{'icon-gouxuan':isSelectAll[index] && isSelectAll[index].child[j],'icon-checkboxround0':isSelectAll[index] && !isSelectAll[index].child[j]}"></i>
                                </div>
                                {{second.name}}
                            </div>
                            <ul class="item_fourth" v-show="isOpenItem[index] && isOpenItem[index].child[j]">
                                <li v-for="(third,k) in second.member" :key="third.pid">
                                    <input @click="stateChanged(index, j, k)" type="checkbox" name="school"
                                           :id="'check'+third.pid"
                                           v-model="selectPeople"
                                           :value="third">
                                    <label class="content-wrap" :for="'check'+third.pid">
                                        <div class="item_img" :style="{ background: third.headColor }">{{ third.name }}</div>
                                        <div class="content">
                                            <p class="content_name">
                                                {{third.userName}}
                                            </p>
                                            <p class="vice">{{ third.job }}</p>
                                        </div>
                                    </label>
                                </li>
                            </ul>
                        </div>
                        <li v-for="(people,i) in item.member" :key="people.pid">
                            <input @click="stateChanged(index,i)" type="checkbox" name="school" :id="'check'+people.pid"
                                   v-model="selectPeople"
                                   :value="people">
                            <label class="content-wrap" :for="'check'+people.pid">
                                <div class="item_img" :style="{ background: people.headColor }">{{ people.name }}</div>
                                <div class="content">
                                    <p class="content_name">
                                        {{people.userName}}
                                    </p>
                                    <p class="vice">{{ people.job }}</p>
                                </div>
                            </label>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>

    export default {
        name: "demo",
        data() {
            return {
                headColor: ['#1c71fb', '#f7aa47', '#00c182', '#ff6769', '#917ee6', '#2cb2eb'],//待选颜色
                jobList: [{
                    "id": "2511",
                    "name": "嫩江第一中学",
                    "member": [{
                        "pid": "12058",
                        "userName": "冷风",
                        "job": "安全主任",
                        "name": "冷风"
                    }, {
                        "pid": "12005",
                        "userName": "周大龙",
                        "job": "安全主任",
                        "name": "大龙"
                    }],
                    "son": [{
                        "id": "2513",
                        "name": "校领导",
                        "member": [{
                            "pid": "12056",
                            "userName": "凌凌",
                            "job": "安全主任",
                            "name": "凌凌"
                        }, {
                            "pid": "12039",
                            "userName": "唐老师",
                            "job": "安全主任",
                            "name": "老师"
                        }]
                    }]
                }, {
                    "id": "2510",
                    "name": "黑龙江黑河市嫩江县教育局",
                    "son": [{
                        "id": "2525",
                        "name": "办公室 ",
                        "member": [{
                            "pid": "12006",
                            "userName": "张喵喵",
                            "job": "安全主任",
                            "name": "喵喵"
                        }, {
                            "pid": "12024",
                            "userName": "张徳龙",
                            "job": "老师",
                            "name": "徳龙"
                        }]
                    }, {
                        "id": "2524",
                        "name": "局长部",
                        "member": [{
                            "pid": "12015",
                            "userName": "小组长",
                            "job": "安全主任",
                            "name": "组长"
                        }, {
                            "pid": "12025",
                            "userName": "TSY",
                            "job": "11",
                            "name": "SY"
                        }]
                    }]
                }, {
                    "id": "2545",
                    "name": "城市之星2总部",
                    "member": [{
                        "pid": "12076",
                        "userName": "文明",
                        "job": "前端开发工程师",
                        "name": "文明"
                    }, {
                        "pid": "12077",
                        "userName": "不文明",
                        "job": "高级架构师",
                        "name": "文明"
                    }]
                }], //从后台获取的人员列表信息
                selectPeople: [],//存储被选择的人员
                isOpenItem: [],//控制每级面板的打开与折叠
                isSelectAll: [],//控制每级面板的选中状态
            }
        },
        mounted() {
            this.initData();//数据初始化
        },
        methods: {
            checkItem(index) {//展开或折叠面板
                this.$set(this.isOpenItem[index], 'group', !this.isOpenItem[index].group);
            },
            checkSecondItem(firstIndex, secondIndex) {//展开或折叠第二层面板
                this.$set(this.isOpenItem[firstIndex].child, secondIndex, !this.isOpenItem[firstIndex].child[secondIndex]);
            },
            checkAll(index) {//第一层级的全选操作
                this.$set(this.isSelectAll[index], 'group', !this.isSelectAll[index].group);//改变当前按钮的选中状态
                if (!this.jobList[index].member && !this.jobList[index].son) {
                    return
                }
                if (!this.isSelectAll[index].group) {// 从全选 =》 全不选
                    for (let key in this.isSelectAll[index].child) { // 移除所有第二层级子项的选中状态
                        this.$set(this.isSelectAll[index].child, key, false);
                    }
                    for (let i = 0, len = this.selectPeople.length; i < len; i++) {
                        if (!this.selectPeople[i]) { //删除干净了
                            return
                        }
                        for (let k in this.jobList[index].son) {//循环删除有部门的人员（对应列表第三层级）
                            for (let j in this.jobList[index].son[k].member) {
                                if (this.selectPeople[i] && this.selectPeople[i].pid == this.jobList[index].son[k].member[j].pid) {
                                    this.selectPeople.splice(i, 1);
                                    i--;
                                    break;
                                }
                            }
                        }
                        for (let j in this.jobList[index].member) {//循环删除没有部门的人员（对应列表第二层级）
                            if (this.selectPeople[i] && this.selectPeople[i].pid == this.jobList[index].member[j].pid) {
                                this.selectPeople.splice(i, 1);
                                i--;
                                break;
                            }
                        }
                    }
                } else {// 从全不选 =》 全选
                    for (let key in this.isSelectAll[index].child) { // 给所有第二层级子项添加选中状态
                        this.$set(this.isSelectAll[index].child, key, true);
                    }
                    for (let i in this.jobList[index].member) {//循环添加没有部门的人员（对应列表第二层级）
                        if (this.selectPeople.includes(this.jobList[index].member[i])) { //如果已经存在，就不用再进行添加
                            continue;
                        }
                        this.selectPeople.push(this.jobList[index].member[i]);
                    }
                    for (let i in this.jobList[index].son) {//循环添加有部门的人员（对应列表第三层级）
                        for (let j in this.jobList[index].son[i].member) {
                            if (this.selectPeople.includes(this.jobList[index].son[i].member[j])) { //如果已经存在，就不用再进行添加
                                continue;
                            }
                            this.selectPeople.push(this.jobList[index].son[i].member[j]);
                        }
                    }
                }
            },
            checkSecondAll(index, secondIndex) {//第二层级的全选操作
                this.$set(this.isSelectAll[index].child, secondIndex, !this.isSelectAll[index].child[secondIndex]);//改变当前按钮的选中状态
                let members = this.jobList[index].son[secondIndex].member;//当前选项的所有成员
                if (!members) {
                    return
                }
                if (!this.isSelectAll[index].child[secondIndex]) {// 从全选 =》 全不选
                    this.$set(this.isSelectAll[index], 'group', false);//改变顶级按钮的选中状态为非选中状态
                    for (let i = 0, len = this.selectPeople.length; i < len; i++) {
                        if (!this.selectPeople[i]) { //删除干净了
                            return
                        }
                        for (let j in members) {//循环删除当前部门的所有人员（对应列表第三层级）
                            if (this.selectPeople[i].pid == members[j].pid) {
                                this.selectPeople.splice(i, 1);
                                i--;
                                break;
                            }
                        }
                    }
                } else {// 从全不选 =》 全选
                    for (let j in members) {//循环添加当前部门的所有人员（对应列表第三层级）
                        if (this.selectPeople.includes(members[j])) { //如果已经存在，就不用再进行添加
                            continue;
                        }
                        this.selectPeople.push(members[j]);
                    }
                    this.setFirstLevelChecked(index);
                }
            },
            setHeadColor(data, headColor) {//设置头像背景颜色
                let length = headColor.length;
                for (let i in data) {
                    //添加headColor属性
                    this.$set(data[i], 'headColor', headColor[Math.floor(Math.random() * length)]);
                }
            },
            stateChanged(index, i, j) {
                if (j !== undefined) { //如果有j值，代表第三层级数据
                    if (this.selectPeople.includes(this.jobList[index].son[i].member[j])) {//点击之前为选中状态
                        this.$set(this.isSelectAll[index].child, i, false);//改变父级按钮的选中状态为非选中状态
                        this.$set(this.isSelectAll[index], 'group', false);//改变顶级按钮的选中状态为非选中状态
                    } else {//点击之前为非选中状态
                        //给父级添加选中状态
                        for (let k = 0; k < this.jobList[index].son[i].member.length; k++) {
                            if (!this.selectPeople.includes(this.jobList[index].son[i].member[k]) && this.jobList[index].son[i].member[k] != this.jobList[index].son[i].member[j]) {//只要有其中一个未选中，就跳出循环，不给父级添加选中状态
                                return false
                            }
                        }
                        this.$set(this.isSelectAll[index].child, i, true);//改变父级按钮的选中状态为选中状态
                        this.setFirstLevelChecked(index, this.jobList[index].son[i].member[j]);//给第一级添加选中状态
                    }
                } else {//没有j值，第二层级数据
                    if (this.selectPeople.includes(this.jobList[index].member[i])) {//点击之前为选中状态
                        this.$set(this.isSelectAll[index], 'group', false);//改变父级按钮的选中状态为非选中状态
                    } else {//点击之前为非选中状态
                        this.setFirstLevelChecked(index, this.jobList[index].member[i]);//给第一级添加选中状态
                    }
                }
            },
            setFirstLevelChecked(index, data) {//给第一级添加选中状态
                for (let k in this.jobList[index].member) {
                    if (!this.selectPeople.includes(this.jobList[index].member[k]) && data != this.jobList[index].member[k]) {//只要有其中一个未选中，就跳出循环，不给父级添加选中状态
                        return false
                    }
                }
                for (let i in this.jobList[index].son) {//循环添加有部门的人员（对应列表第三层级）
                    for (let j in this.jobList[index].son[i].member) {
                        if (!this.selectPeople.includes(this.jobList[index].son[i].member[j]) && data != this.jobList[index].son[i].member[j]) { //如果已经存在，就不用再进行添加
                            return false
                        }
                    }
                }
                this.$set(this.isSelectAll[index], 'group', true);//改变第一级按钮的选中状态为选中状态
            },
            initData() {//数据初始化
                //设置头像背景颜色
                let len = this.jobList.length;
                for (let i = 0; i < len; i++) {
                    this.setHeadColor(this.jobList[i].member, this.headColor);
                    //根据数据初始化全选按钮状态
                    this.isSelectAll.push([]);
                    this.$set(this.isSelectAll[i], 'group', false);
                    this.$set(this.isSelectAll[i], 'child', []);
                    //根据数据初始化折叠面板折叠状态
                    this.isOpenItem.push([]);
                    this.$set(this.isOpenItem[i], 'group', false);
                    this.$set(this.isOpenItem[i], 'child', []);
                    //设置第二层级的状态
                    for (let j in this.jobList[i].son) {
                        this.isSelectAll[i].child.push(false)
                        this.isOpenItem[i].child.push(false)
                        this.setHeadColor(this.jobList[i].son[j].member, this.headColor);
                    }
                }
            }
        }
    }
</script>

<style lang="stylus" scoped>
    @import "./style/reset.styl"
    @import "./style/common.styl"

    .wrapper {
        padding-bottom: px2vw(90px)
    }

    .item_first {
        borderBottom()
    }

    .item {
        display flex
        fontSize(34px)
        paddingAround()
        .iconfont {
            fontSize(44px)
        }
    }

    .item_second li {
        display: flex
        align-items center
        borderTop()
        paddingAround()
        padding-left px2vw(45px)
        .content-wrap {
            display: flex;
            width: 100%
            .item_img {
                display flex
                justify-content center
                align-items center
                width: px2vw(90px)
                height: px2vw(90px)
                border-radius: 50%
                margin-left px2vw(20px)
                margin-right px2vw(15px)
                color: #fff
                text-align: center
            }
            .content {
                .content_name {
                    font-weight: bold
                }
                .vice {
                    color: vice-font-color
                }
            }

        }

    }

    .item_second label {
        flex 1
    }

    .item_third {
        borderTop()
    }

    .item_third .item {
        margin-left px2vw(15px)
    }

    .item_fourth > li {
        padding-left px2vw(90px)
    }

    .icon-gouxuan {
        color: main-color
    }

    .icon-checkboxround0 {
        color: vice-font-color
    }

    .checkGroup {
        width px2vw(60px)
        display inline-block
    }

    .head_img {
        width: px2vw(100px)
        height: px2vw(100px)
        border-radius 50%
        margin-left px2vw(15px)
        margin-right px2vw(15px)
    }

    input[type='checkbox'] {
        width: px2vw(44px);
        height: px2vw(44px);
        -webkit-appearance: none;
        background: url("./assets/checkbox_round.png") no-repeat center;
        background-size px2vw(44px) px2vw(44px)
        outline: none;
    }

    input[type='checkbox']:checked {
        background: url("./assets/checkbox_check.png") no-repeat center;
        background-size px2vw(44px) px2vw(44px)
    }
</style>
