<!-- IDContent.svelte -->
<script>
  export let documentTitle = "";
  export let completeName = "";
  export let birthDate = "";
  export let address = "";
  export let weight = "";
  export let height = "";
  export let civilStatus = "";
  export let IDpictureUrl = "";
  export let signatureURL = "";
  export let mywindow = null;
  export let emergency_contact = "";
  export let emergency_name = "";
  export let gender = "";

  import bg from "./images/id_background.jpeg";
  import bg2nd from "./images/id_back.jpeg";

  const handleEvent = () => {
    mywindow.print();
    // mywindow.close();
  };
  setTimeout(handleEvent, 1500);

  function convertDateFormat(inputDate) {
    // Parse the input date string
    const parts = inputDate.split("-");
    const year = parseInt(parts[0]);
    const month = parseInt(parts[1]);
    const day = parseInt(parts[2]);

    // Create a Date object with the parsed values
    const dateObject = new Date(year, month - 1, day);

    // Get month name
    const monthNames = [
      "January",
      "February",
      "March",
      "April",
      "May",
      "June",
      "July",
      "August",
      "September",
      "October",
      "November",
      "December",
    ];
    const monthName = monthNames[dateObject.getMonth()];

    // Format the output date string
    // const outputDate = `${monthName} ${day}, ${year}`;

    return [monthName, day, year];
  }

  function getOrdinalSuffix(number) {
    // Check for special cases: 11th, 12th, 13th
    if (number % 100 >= 11 && number % 100 <= 13) {
      return "th";
    }

    // Regular cases
    switch (number % 10) {
      case 1:
        return "st";
      case 2:
        return "nd";
      case 3:
        return "rd";
      default:
        return "th";
    }
  }
</script>

<div class="box">
  <style>
    .box {
      width: 799px;
      height: 496px;
    }

    .box .group {
      position: fixed;
      width: 815px;
      height: 496px;
      top: 0;
      left: 0;
    }

    .box .overlap-group {
      position: relative;
      width: 799px;
      height: 496px;
      background-size: cover;
      background-position: 50% 50%;
    }

    .box .profile-ID {
      position: absolute;
      width: 170px;
      height: 185px;
      top: 140px;
      left: 12px;
      object-fit: cover;
    }

    .box .text-wrapper {
      position: absolute;
      width: 502px;
      height: 40px;
      top: 164px;
      left: 193px;
      font-family: "Noto Serif-Bold", Helvetica;
      font-weight: 700;
      color: #113647;
      font-size: 28px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .overlap {
      position: absolute;
      width: 122px;
      height: 55px;
      top: 205px;
      left: 312px;
    }

    .box .div {
      position: absolute;
      width: 122px;
      height: 28px;
      top: 0;
      left: 0;
      font-family: "Noto Serif-Bold", Helvetica;
      font-weight: 700;
      color: #113647;
      font-size: 16px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-2 {
      position: absolute;
      width: 122px;
      height: 28px;
      top: 27px;
      left: 0;
      font-family: "Noto Serif-Bold", Helvetica;
      font-weight: 700;
      color: #113647;
      font-size: 16px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-3 {
      position: absolute;
      width: 122px;
      height: 28px;
      top: 232px;
      left: 585px;
      font-family: "Noto Serif-Bold", Helvetica;
      font-weight: 700;
      color: #113647;
      font-size: 16px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-4 {
      position: absolute;
      width: 122px;
      height: 28px;
      top: 204px;
      left: 585px;
      font-family: "Noto Serif-Bold", Helvetica;
      font-weight: 700;
      color: #113647;
      font-size: 16px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-5 {
      position: absolute;
      width: 199px;
      height: 28px;
      top: 376px;
      left: 288px;
      font-family: "Noto Serif-Bold", Helvetica;
      font-weight: 700;
      color: #113647;
      font-size: 18px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-6 {
      position: absolute;
      width: 199px;
      height: 28px;
      top: 410px;
      left: 300px;
      font-family: "Noto Serif-Bold", Helvetica;
      font-weight: 700;
      color: #113647;
      font-size: 18px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .p {
      position: absolute;
      width: 273px;
      top: 261px;
      left: 312px;
      font-family: "Noto Serif-Bold", Helvetica;
      font-weight: 700;
      color: #113647;
      font-size: 16px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .img {
      position: absolute;
      width: 88px;
      height: 54px;
      top: 376px;
      left: 46px;
      object-fit: cover;
    }

    .second {
      position: fixed;
      width: 815px;
      height: 496px;
      top: 550px;
      left: 0;
    }
  </style>
  <div class="box">
    <div class="group">
      <div class="overlap-group">
        <img class="background" src={bg} />
        <img class="profile-ID" src={IDpictureUrl} />

        <div class="text-wrapper">{completeName}</div>
        <div class="overlap">
          <div class="div">{birthDate}</div>
          <div class="text-wrapper-2">{gender}</div>
        </div>
        <div class="text-wrapper-3">{civilStatus}</div>
        <div class="text-wrapper-4">{weight}kg / {height}cm</div>
        <div class="text-wrapper-5">{emergency_name}</div>
        <div class="text-wrapper-6">{emergency_contact}</div>
        <p class="p">
          {address}
        </p>
        <img class="img" src={signatureURL} />
      </div>
    </div>
    <img src={bg2nd} alt="" class="second"/>
  </div>
</div>
