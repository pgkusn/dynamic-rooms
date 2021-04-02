<template>
    <div class="container">
        <div class="row">
            <div class="col-lg-3 col-md-4 col_edit">
                <h1>飯店資料</h1>
                <hr/>
                <label>折扣</label>
                <input class="form-control" v-model.number="common.discount"/>
                <label class="mt-2">服務費</label>
                <input class="form-control" v-model.number="common.charge"/><br/>
                <h1>房間編輯</h1>
                <select class="form-control" v-model="selectRoom">
                    <option selected disabled>選擇房間</option>
                    <option v-for="room in rooms" :key="room.id" :value="room">{{ room.name }}</option>
                </select>
                <div class="room_edit btn btn-outline-primary" @click="addRoom">+ 新增房間</div>
                <hr/>
                <div class="room_edit">
                    <template v-for="room in rooms" :key="room.id">
                        <div class="room_tag" v-if="room.id === selectRoom.id">
                            <label for="toggle_check">
                                <h4>{{ selectRoom.name }}<i class="fas fa-trash-alt delete_btn" @click.prevent="deleteRoom(selectRoom.id)"></i></h4>
                            </label>
                            <input id="toggle_check" class="toggle_check" type="checkbox"/>
                            <div class="edit_part">
                                <div>
                                    <label>房間名稱</label>
                                    <input
                                        class="form-control"
                                        name="name"
                                        v-model="room.name"
                                    />
                                </div>
                                <div>
                                    <label>定價</label>
                                    <input
                                        class="form-control"
                                        name="price"
                                        v-model.number="room.price"
                                    />
                                </div>
                                <div>
                                    <label>折扣</label>
                                    <input
                                        class="form-control"
                                        name="discount"
                                        v-model.number="room.discount"
                                    />
                                </div>
                                <div>
                                    <label>英文</label>
                                    <input
                                        class="form-control"
                                        name="eng"
                                        v-model="room.eng"
                                    />
                                </div>
                                <div>
                                    <label>圖片網址</label>
                                    <input
                                        class="form-control"
                                        name="cover"
                                        v-model="room.cover"
                                    />
                                </div>
                                <div>
                                    <p class="mb-1">房間設備</p>
                                    <label class="mb-0">
                                        <input
                                            type="checkbox"
                                            v-model="room.equipment.breakfast"
                                        /> 早餐
                                    </label>
                                    <label class="mb-0 ml-2">
                                        <input
                                            type="checkbox"
                                            v-model="room.equipment.wifi"
                                        /> wifi
                                    </label>
                                    <label class="mb-0 ml-2">
                                        <input
                                            type="checkbox"
                                            v-model="room.equipment.bathtub"
                                        /> 浴缸
                                    </label>
                                </div>
                            </div>
                        </div>
                    </template>
                </div>
            </div>
            <div class="col-lg-9 col-md-8 col_rooms">
                <h1>房間列表</h1>
                <hr/>
                <div class="row">
                    <div class="col_room col-lg-4 col-md-6" v-for="room in rooms" :key="room.id">
                        <Room :room="room" :common="common" @deleteRoom="deleteRoom"></Room>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref } from 'vue';
import { cloneDeep } from 'lodash';
import Room from './components/Room.vue';
import roomsData from '@/rooms.json';

export default {
    name: 'App',
    components: {
        Room
    },
    setup () {
        const common = reactive({
            discount: 0.9,
            charge: 200
        });

        const rooms = ref([]);
        rooms.value = cloneDeep(roomsData);

        const selectRoom = ref({});
        selectRoom.value = cloneDeep(rooms.value[0]);

        const addRoom = () => {
            rooms.value.push({
                id: Date.now(),
                name: '新房間',
                eng: 'New Room',
                price: 0,
                amount: 0,
                cover: 'https://fakeimg.pl/195x150/282828/eae0d0/',
                discount: 1,
                equipment: {
                    wifi: false,
                    bathtub: false,
                    breakfast: false
                }
            });
            selectRoom.value = rooms.value[rooms.value.length - 1];
        };

        const deleteRoom = id => {
            const index = rooms.value.findIndex(room => room.id === id);
            rooms.value.splice(index, 1);
            selectRoom.value = rooms.value[0];
        };

        return {
            rooms,
            deleteRoom,
            selectRoom,
            common,
            addRoom
        };
    }
};
</script>

<style>
html,
body {
    margin: 0;
    padding: 0;
}
@media screen and (min-width: 767px) {
    html,
    body {
        overflow: hidden;
        height: 100vh;
    }
}

body {
    padding: 20px;
}

h1 {
    font-size: 24px;
}

.col_edit,
.col_rooms {
    padding-top: 1rem;
    padding-bottom: 2rem;
}
@media screen and (min-width: 767px) {
    .col_edit,
    .col_rooms {
        overflow-y: scroll;
        height: calc(100vh - 40px);
    }
}

.col_edit {
    background-color: #f4f4f4;
}

.room_tag > label {
    width: 100%;
}
.toggle_check {
    display: none;
}
.toggle_check:checked + .edit_part {
    max-height: 500px;
}

.room_edit {
    margin-top: 30px;
    padding: 5px;
}
.room_edit:not(.btn) {
    border-left: solid 4px #ccc;
}
.room_edit h4 {
    display: flex;
    margin: 10px 0 10px 5px;
    padding: 0;
    cursor: pointer;
    justify-content: space-between;
    align-items: center;
}
.room_edit .edit_part {
    overflow: hidden;
    max-height: 0;
}
.room_edit .edit_part > div {
    margin-top: .5rem;
}

.delete_btn {
    font-size: 20px;
    cursor: pointer;
    transition: .5s;
}
.delete_btn:hover {
    color: #db4343;
}

.col_room {
    padding: 20px;
}

.room_container {
    box-shadow: 0 0 10px rgba(0, 0, 0, .3);
    transition: .5s;
}
.room_container:hover .cover {
    background-size: auto 110%;
}
@media screen and (max-width: 767px) and (min-width: 335px) {
    .room_container:hover .cover {
        background-size: 110% auto;
    }
}
.room_container:hover .cover .delete_btn,
.room_container:hover .cover .icon {
    opacity: 1;
}
.room_container .cover {
    position: relative;
    height: 150px;
    background-color: #eee;
    background-position: center center;
    background-size: auto 100%;
    transition: .5s;
}
@media screen and (max-width: 767px) and (min-width: 335px) {
    .room_container .cover {
        background-size: 100% auto;
    }
}
.room_container .cover h3 {
    position: absolute;
    bottom: 10px;
    padding: 5px 15px;
    background-color: rgba(255, 255, 255, .8);
    font-size: 20px;
}
.room_container .cover .delete_btn {
    position: absolute;
    top: 10px;
    left: 10px;
    z-index: 2;
    color: white;
    opacity: 0;
}
.room_container .cover .delete_btn:hover {
    color: #db4343;
}
.room_container .cover .icon {
    position: absolute;
    top: 5px;
    right: 0;
    color: white;
    text-shadow: 0 0 5px rgba(0, 0, 0, .5);
    letter-spacing: 7px;
    opacity: .5;
    transition: .5s;
}
.room_container .info {
    padding: 10px;
}
.room_container .info h5 {
    font-size: 14px;
}
.room_container .info .final_price {
    float: right;
    color: #db4343;
}
.room_container .info .price:before {
    content: '$';
}
.room_container .info p {
    color: #888;
    text-align: right;
    font-size: 10px;
}
</style>
