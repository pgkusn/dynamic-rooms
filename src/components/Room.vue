<template>
    <div class="room_container">
        <div class="cover" :style="{ 'background-image': `url('${$props.room.cover}')` }">
            <i class="fas fa-times delete_btn" @click="$emit('deleteRoom', $props.room.id);"></i>
            <h3>{{ $props.room.name }}</h3>
            <div class="icon">
                <i class="fas fa-coffee" v-if="$props.room.equipment.breakfast"></i>
                <i class="fas fa-wifi" v-if="$props.room.equipment.wifi"></i>
                <i class="fas fa-bath" v-if="$props.room.equipment.bathtub"></i>
            </div>
        </div>
        <div class="info">
            <h5>{{ $props.room.eng }}</h5>
            <h5>{{ $props.room.discount }} x {{ $props.common.discount }} = {{ totalDiscount }} 折</h5>
            <h4>
                <s class="price">{{ $props.room.price }}</s><span class="price final_price">{{ totalPrice }}</span>
                <p>(含服務費 ${{ $props.common.charge }})</p>
            </h4>
        </div>
    </div>
</template>

<script>
import { computed } from 'vue';
export default {
    name: 'Room',
    props: {
        room: {
            type: Object,
            default: () => {}
        },
        common: {
            type: Object,
            default: () => {}
        }
    },
    setup (props) {
        const totalDiscount = computed(() => Math.floor(props.room.discount * props.common.discount * 100));
        const totalPrice = computed(() => Math.floor(props.room.price * props.room.discount * props.common.discount) + props.common.charge);

        return {
            totalDiscount,
            totalPrice
        };
    }
};
</script>