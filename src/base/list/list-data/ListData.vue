<template>
    <div class="songslist-con">
        <ul class="songslist-con-ul">
            <li v-for="(item,index) in songsdata.tracks" :key="index">  
                <div class="songslist-con-box"  @click="toPlayer(item,index)">
                    <span class="songslist-title">{{ item.name }}</span>
                    <span>
                        {{ item.ar[0].name + ((item.ar[1] && item.ar[1].name)?' / '+item.ar[1].name : '') + (item.al?' - '+item.al.name:"") }}
                    </span>
                </div>
                <span class="songslist-num">{{ index+1 }}</span>
                <span class="songslist-more" @click="More(item)">
                    <img src="@/assets/img/more2.png">
                </span>                
            </li>
        </ul>
    </div>
</template>
<script>
import {mapMutations,mapGetters} from 'vuex'
export default{
    props: {
        songsdata:{
            type: Object,
            default:{},
        }
    },
    computed:{
        ...mapGetters([
            'originList',
            'song'
        ])
    },
    methods:{
        ...mapMutations([
            'filterSong',
            'setSonglist',
            'setplayerIndex',
            'showMore',
            'setOriginList'
        ]),
        // 跳到播放器 并且播放
        toPlayer(song,index){            
            // 当前歌曲数组
            localStorage.setItem('originlist',JSON.stringify(this.songsdata.tracks));
            localStorage.setItem('songlist',JSON.stringify(this.songsdata.tracks));
            localStorage.setItem('songindex',index);
            this.setOriginList(this.songsdata.tracks);
            this.setSonglist(this.songsdata.tracks);
            this.setplayerIndex(index);
            this.$router.push({path:'/player/'+song.id});
        },
        // 点击右侧获取更多
        More(song){
            // 过滤每一首歌信息
            this.filterSong(song);
            this.$store.dispatch('getSongComment',song.id);
            // 显示遮罩
            this.showMore();      
        }
    },
}
</script>
<style>
@import './list.css';
.animated {animation-duration: 0.3s;}
</style>

