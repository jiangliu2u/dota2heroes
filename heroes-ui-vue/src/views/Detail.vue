<template>
    <div class="about">
        <h1></h1>
        <el-container>
            <el-header>{{detail.name_cn}}({{level}}级)</el-header>
            <el-main>
                <div class="grid-content">

                    <el-slider v-model="level" :min="1" :max="25"></el-slider>
                    <div class="el-button--danger stat">力量:{{current.str}}</div>
                    <div class="el-button--success stat">敏捷:{{current.agi}}</div>
                    <div class="el-button--primary stat">智力:{{current.int}}</div>
                    <div class="el-button--warning stat">攻击力:{{current.attack_min}}-{{current.attack_max}}</div>
                    <div class="stat">生命:{{current.hp}}</div>
                    <div class="stat">魔法:{{current.mana}}</div>
                    <div class="stat">护甲:{{current.armor}}</div>
                    <div class="stat">移速:{{detail.move_speed}}</div>
                    <el-card class="box-card">
                        <div slot="header" class="clearfix">
                            <span class="title">天赋</span>
                            <!--//<el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button>-->
                        </div>
                        <div class="text item">

                            <div v-for="(line,index) in raw.talent">
                                <div>{{index}}级:&nbsp&nbsp&nbsp{{line}}</div>
                            </div>
                        </div>
                    </el-card>
                    <div v-for="(item,key) in raw.abilities">
                        <el-card class="box-card">
                            <div slot="header" class="clearfix">
                                <span class="title">{{key}}</span>
                                <!--//<el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button>-->
                            </div>
                            <div class="text item">
                                <div class="desc">{{item.desc}}</div>
                                <div v-for="(line,index) in item.detail">
                                    <div>{{line}}</div>
                                </div>
                            </div>
                        </el-card>
                    </div>
                </div>
            </el-main>
        </el-container>

    </div>
</template>
<script>
    export default {
        name: "detail",
        data() {
            return {
                detail: {
                    name_cn: "",
                    str: 0,
                    str_add: 0,
                    agi: 0,
                    agi_add: 0,
                    int: 0,
                    int_add: 0,
                    armor: 0,
                    armor_init: 0,
                    move_speed: 0,
                    hp: 0,
                    mana: 0,
                    attack_min: 0,
                    attack_max: 0,
                },
                current: {
                    str: 0,
                    agi: 0,
                    int: 0,
                    armor: 0,
                    move_speed: 0,
                    hp: 0,
                    mana: 0,
                    attack_min: 0,
                    attack_max: 0,
                    attack_add: 0,
                    attack_init: 0,

                },
                level: 1,
                raw: {},

            }
        },
        methods: {
            calculate() {
                console.log((Number(this.detail.armor_init) + Number(this.detail.agi) / 6));
                this.current.armor = Math.floor((Number(this.detail.armor_init) + (Number(this.detail.agi) + (this.level - 1) * Number(this.detail.agi_add)) * 0.16) * 10) / 10;
                this.current.hp = Math.floor(200 + (Number(this.detail.str) + (this.level - 1) * Number(this.detail.str_add)) * 20);
                this.current.mana = 75 + Math.floor((Number(this.detail.int) + (this.level - 1) * Number(this.detail.int_add)) * 12);
                this.current.agi = Math.floor(Number(this.detail.agi) + (this.level - 1) * Number(this.detail.agi_add));
                this.current.str = Math.floor(Number(this.detail.str) + (this.level - 1) * Number(this.detail.str_add));
                this.current.int = Math.floor(Number(this.detail.int) + (this.level - 1) * Number(this.detail.int_add));
                this.current.attack_min = Number(this.current.attack_init) + Math.floor(Number(this.detail.attack_min) + (this.level - 1) * Number(this.current.attack_add));
                this.current.attack_max = Number(this.current.attack_init) + Math.floor(Number(this.detail.attack_max) + (this.level - 1) * Number(this.current.attack_add));
            }
        },
        mounted() {
            this.raw = this.$route.params['detail'];
            for (let i in this.raw['abilities']) {
                if (this.raw['abilities'].hasOwnProperty(i)) {
                    this.raw['abilities'][i]['detail'] = this.raw['abilities'][i]['detail'].split(",");
                }
            }
            this.detail.name_cn = this.raw['name_cn'];
            this.detail.str = this.raw['attr']['力量'];
            this.detail.str_add = this.raw['attr']['str_add'];
            this.detail.agi = this.raw['attr']['敏捷'];
            this.detail.agi_add = this.raw['attr']['agi_add'];
            this.detail.int = this.raw['attr']['智力'];
            this.detail.int_add = this.raw['attr']['int_add'];
            this.detail.armor = this.raw['attr']['护甲'];
            this.detail.armor_init = this.raw['attr']['armor_init'];
            this.detail.move_speed = this.raw['attr']['移动速度'];
            this.detail.hp = this.raw['attr']['hp'];
            this.detail.mana = this.raw['attr']['mana'];
            this.detail.attack_min = this.raw['attr']['attack_min'];
            this.detail.attack_max = this.raw['attr']['attack_max'];
            console.log(this.raw.type)
            if (this.raw["type"] === "str") {
                this.current.attack_add = this.detail.str_add;
                this.current.attack_init = this.detail.str;
            } else if (this.raw["type"] === "agi") {
                this.current.attack_add = this.detail.agi_add;
                this.current.attack_init = this.detail.agi;
            } else {
                this.current.attack_add = this.detail.int_add;
                this.current.attack_init = this.detail.int;
            }
            console.log(this.raw);
            console.log(this.detail);
            this.calculate()
        },
        watch: {
            level: function (val) {
                this.calculate()
            }
        }
    }
</script>
<style>
    .stat {
        margin: 10px;
        border-radius: 4px;
    }
    .desc{
        color: #606266;
    }
    .title{
        color: coral;
    }
</style>
