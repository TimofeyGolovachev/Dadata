<script defer>
window.addEventListener('DOMContentLoaded', () => {
  let url = "https://suggestions.dadata.ru/suggestions/api/4_1/rs/suggest/address";

  let form = document.querySelector('form');
  let city = document.querySelector('.input-city');
  let street = document.querySelector('.input-street');
  let house = document.querySelector('.input-house');

  let modal = document.querySelector('.modal');
  let modalBtn = document.querySelector('.modal-btn');
  let modalInput = document.querySelector('.modal-input');

  let token;
  modalBtn.setAttribute("disabled", "true")

  modalInput.addEventListener("input", ButtonED);

  function ButtonED() {
    if (modalInput.value === "") {
      modalBtn.disabled = true;
    } else {
      modalBtn.disabled = false;
    }
  }

  modalBtn.addEventListener('click', () => {
    token = modalInput.value;
    alert(token);
    modal.classList.add('none');
    form.classList.remove('none');
  })





  async function load(query) {
    const response = await fetch(url,
        {
          method: "POST",
          mode: "cors",
          headers: {
            "Content-Type": "application/json",
            "Accept": "application/json",
            "Authorization": "Token " + token
          },
          body: JSON.stringify({query: query})
        });
    const data = await response.json();
    return data;
  }

  let container = document.querySelector('.container');
  let btn = document.querySelector('.btn');


  btn.addEventListener('click', (e) => {
    e.preventDefault();
    let address = city.value + " " + street.value + " " + house.value;
    container.innerHTML = '';
    let list = document.createElement('ul');
    list.classList.add('list');
    load(address).then(
        function (result) {
          for (let i = 0; i < result.suggestions.length; i++) {
            let el = document.createElement('li');
            el.classList.add('element');
            el.append(result.suggestions[i].value);
            list.append(el);
          }

        }
    );

    container.append(list);
  })


});
</script>

<template>
  <div class="modal">
    <span class="modal-title">
      Введите токен:
    </span>
    <input class="modal-input" type="text" placeholder="Токен:">
    <button class="modal-btn">
      Принять
    </button>
  </div>
  <form class="form none">
    <label class="label-input">
      Введите город:
      <input class="input-city input" type="search">
    </label>
    <label class="label-input">
      Введите улицу:
      <input class="input-street input" type="search">
    </label>
    <label class="label-input">
      Введите номер дома:
      <input class="input-house input" type='search'>
    </label>
    <button class="btn">
      Поиск
    </button>
  </form>
  <div class="container">

  </div>
</template>

<style>
.active {
  display: block;
}

.modal {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  flex-direction: column;
  border: 1px solid black;
  background-color: darkgray;
  align-items: center;
  padding: 20px 10px;
  height: 100px;
  justify-content: space-between;
  width: 290px;
}

.modal-btn {
  padding: 5px 10px;
  background-color: white;
  font-size: 14px;
  cursor: pointer;

}

.modal-input {
  height: 25px;
  padding: 0px 5px;
  width: 100%;
}

.list {
  list-style: none;
  max-width: 500px;
}

.element {
  min-height: 20px;
  width: 100%;
  padding: 10px 10px;
  border: 1px solid black;
}

.input {
  width: 100%;
  height: 30px;
  padding: 10px 10px;
}

.element:not(:last-child) {
  margin-bottom: 10px;
}

.form {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  margin-right: 50px;
  max-height: 250px;

}

.label-input {
  display: flex;
  flex-direction: column;
  margin-bottom: 10px;
  height: 50px;
}

.btn {
  width: 50%;
  align-self: center;
  padding: 10px 5px;
  cursor: pointer;
}

.none {
  display: none;
}


@media (max-width: 670px) {
  .form {
    margin-bottom: 20px;
    margin-right: 0;

  }

}
</style>
