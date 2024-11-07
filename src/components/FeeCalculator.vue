<template>
    <div>
        <h2>会費計算ツール</h2>
        <div class="container">
            <label>全人数: <input type="number" v-model.number="totalPeople" min="1" /></label>
            <label>金額 (円): <input type="number" v-model.number="amountPerPerson" min="1" /></label>
            <label>新入社員人数: <input type="number" v-model.number="newEmployees" min="0" /></label>
            <label>会費の上限 (円): <input type="number" v-model.number="maxFee" min="1" /></label>
            <button @click="calculateOptimalGiftCards">最適なギフトカード枚数を計算</button>
            <div v-if="feePerPerson !== null">
                <p>一人当たりの会費: {{ feePerPerson }} 円</p>
                <p>必要なギフトカードの枚数: {{ giftCardCount }}</p>
            </div>
        </div>

        <h2>指定されたギフトカード枚数での計算</h2>
        <div class="container">
            <label>合計人数: <input type="number" v-model.number="totalPeopleManual" min="1" /></label>
            <label>金額 (円): <input type="number" v-model.number="amountPerPersonManual" min="1" /></label>
            <label>新入社員人数: <input type="number" v-model.number="newEmployeesManual" min="0" /></label>
            <label>ギフトカード枚数: <input type="number" v-model.number="giftCardCountManual" min="0" /></label>
            <button @click="calculateManualFee">計算する</button>

            <div v-if="manualFeePerPerson !== null">
                <p>既存社員一人当たりの支払金額: {{ manualFeePerPerson }} 円</p>
            </div>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
setup() {
    // 最適なギフトカード枚数の計算用
    const totalPeople = ref(0);
    const amountPerPerson = ref(0);
    const newEmployees = ref(0);
    const maxFee = ref(7700);
    const feePerPerson = ref(null);
    const giftCardCount = ref(0);

    // 手動入力計算用
    const totalPeopleManual = ref(0);
    const amountPerPersonManual = ref(0);
    const newEmployeesManual = ref(0);
    const giftCardCountManual = ref(0);
    const manualFeePerPerson = ref(null);

    const GIFT_CARD_VALUE = 500;

    // 最適なギフトカード枚数を計算する関数
    const calculateOptimalGiftCards = () => {
    const existingMembers = totalPeople.value - newEmployees.value;
    giftCardCount.value = 0;

    if (existingMembers <= 0) {
        alert("既存人数が無効です。全人数は新入社員人数より多くする必要があります。");
        return;
    }

    let calculatedFeePerPerson;

    // ギフトカード枚数を増やしながら個人の会費が上限以下になるまで計算
    do {
        const totalFee = (totalPeople.value * amountPerPerson.value) - (giftCardCount.value * GIFT_CARD_VALUE);
        calculatedFeePerPerson = Math.floor(totalFee / existingMembers);

        if (calculatedFeePerPerson > maxFee.value) {
        giftCardCount.value++;
        }

    } while (calculatedFeePerPerson > maxFee.value);

    feePerPerson.value = calculatedFeePerPerson;
    };

    // 手動で入力したギフトカード枚数に基づく計算
    const calculateManualFee = () => {
    const existingMembersManual = totalPeopleManual.value - newEmployeesManual.value;

    if (existingMembersManual <= 0) {
        alert("既存人数が無効です。全人数は新入社員人数より多くする必要があります。");
        return;
    }

    // 総額から指定したギフトカード枚数分を引いた後、既存社員で割る
    const totalFeeManual = (totalPeopleManual.value * amountPerPersonManual.value) - (giftCardCountManual.value * GIFT_CARD_VALUE);
    manualFeePerPerson.value = Math.floor(totalFeeManual / existingMembersManual);
    };

    return {
    totalPeople,
    amountPerPerson,
    newEmployees,
    maxFee,
    feePerPerson,
    giftCardCount,
    calculateOptimalGiftCards,

    totalPeopleManual,
    amountPerPersonManual,
    newEmployeesManual,
    giftCardCountManual,
    manualFeePerPerson,
    calculateManualFee,
    };
}
};
</script>

<style scoped>
h2 {
  padding: 1rem 2rem;
  border-left: 5px solid #000;
  background: #f4f4f4;
}
.container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 90%;
    margin: 0 auto;
    gap: .5rem;
}
label {
display: block;
}
input {
margin-left: .5rem;
}
button {
    background-color: skyblue;
  border: none;
  color: white;
  padding: .5rem;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
}
</style>
