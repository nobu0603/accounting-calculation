<template>
    <div>
        <h2>会費とグルメゾンギフトカードの計算</h2>

        <label>全人数: <input type="number" v-model.number="totalPeople" min="1"></label>
        <label>金額 (円): <input type="number" v-model.number="amountPerPerson" min="1"></label>
        <label>新入社員人数: <input type="number" v-model.number="newEmployees" min="0"></label>
        <label>会費の上限 (円): <input type="number" v-model.number="maxFee" min="1"></label>

        <button @click="calculate">計算する</button>

        <h3>計算結果</h3>
        <p>{{ resultMessage }}</p>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
name: 'FeeCalculator',
setup() {
    const totalPeople = ref(null);
    const amountPerPerson = ref(null);
    const newEmployees = ref(null);
    const maxFee = ref(7700);
    const resultMessage = ref("");

    const calculate = () => {
    const giftCardValue = 500;

    if (
        isNaN(totalPeople.value) || isNaN(amountPerPerson.value) || isNaN(newEmployees.value) || isNaN(maxFee.value) ||
        totalPeople.value <= 0 || amountPerPerson.value <= 0 || newEmployees.value < 0 || maxFee.value <= 0 || newEmployees.value >= totalPeople.value
    ) {
        resultMessage.value = "入力が正しくありません。全人数は1以上、新入社員人数は0以上で、全人数より少なくしてください。";
        return;
    }

    let existingMembers = totalPeople.value - newEmployees.value;
    let giftCardCount = 0;
    let feePerPerson = 0;

    // ギフトカード枚数を増やしながら会費が上限以下になるまで計算
    while (feePerPerson > maxFee.value || giftCardCount === 0) {
    let totalFee = (totalPeople.value * amountPerPerson.value) - (newEmployees.value * amountPerPerson.value) - (giftCardCount * giftCardValue);
    feePerPerson = Math.floor(totalFee / existingMembers);

    if (feePerPerson <= maxFee.value) {
        break;
    }

    giftCardCount++;
    }


    resultMessage.value = `会費 (1人あたり): ${feePerPerson}円\n必要なグルメゾンギフトカードの枚数: ${giftCardCount}枚`;
    };

    return {
    totalPeople,
    amountPerPerson,
    newEmployees,
    maxFee,
    resultMessage,
    calculate
    };
}
}
</script>

<style scoped>
label {
display: block;
margin: 8px 0;
}
input {
margin-left: 10px;
}
</style>
