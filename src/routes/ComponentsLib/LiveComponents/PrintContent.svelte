<!-- PrintContent.svelte -->
<script>
  export let documentTitle = "";
  export let mywindow = "";
  export let completeName = "";
  export let address = "";
  export let age = "";
  export let dateOfAppointment = "";
  export let purpose = "";
  export let gender = "";
  export let civilStatus = "";

  import nicolasLogo from "./images/nicolas.jpeg";
  import bacoorLogo from "./images/bacoor.jpeg";
  import clerk from "./images/clerk.png";
  import footer from "./images/footer.png";

  const handleEvent = () => {
    mywindow.print();
    // mywindow.close();
  };
  setTimeout(handleEvent, 1000);

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
      "Jan",
      "Feb",
      "Mar",
      "Apr",
      "May",
      "Jun",
      "Jul",
      "Aug",
      "Sep",
      "Oct",
      "Nov",
      "Dec",
    ];
    const monthName = monthNames[dateObject.getMonth()];

    // Format the output date string
    const outputDate = `${monthName} ${day}, ${year}`;

    return outputDate;
  }
</script>

<div class="box">
  <style>
    .box {
      width: 631px;
      height: 891px;
      margin: 0 auto;
    }

    .box .group {
      position: fixed;
      width: 649px;
      height: 891px;
      top: 0;
      left: 0;
    }

    .box .overlap {
      position: relative;
      width: 631px;
      height: 891px;
    }

    .box .rectangle {
      position: absolute;
      width: 630px;
      height: 891px;
      top: 0;
      left: 0;
      background-color: #ffffff;
    }

    .box .screenshot {
      position: absolute;
      width: 631px;
      height: 225px;
      top: 666px;
      left: 0;
      object-fit: cover;
    }

    .box .img {
      position: absolute;
      width: 93px;
      height: 44px;
      top: 578px;
      left: 269px;
      object-fit: cover;
    }

    .box .text-wrapper {
      position: absolute;
      width: 219px;
      height: 19px;
      top: 341px;
      left: 242px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .div {
      position: absolute;
      width: 322px;
      top: 375px;
      left: 223px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .p {
      position: absolute;
      width: 322px;
      top: 436px;
      left: 223px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .given-this-day-jan {
      position: absolute;
      width: 203px;
      top: 482px;
      left: 223px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-2 {
      position: absolute;
      width: 203px;
      top: 595px;
      left: 202px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-3 {
      position: absolute;
      width: 296px;
      height: 15px;
      top: 462px;
      left: 225px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .overlap-group-wrapper {
      position: absolute;
      width: 113px;
      height: 24px;
      top: 487px;
      left: 68px;
    }

    .box .overlap-group {
      position: relative;
      width: 109px;
      height: 24px;
    }

    .box .text-wrapper-4 {
      position: absolute;
      width: 108px;
      top: 0;
      left: 1px;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #000000;
      font-size: 12px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-5 {
      width: 108px;
      top: 14px;
      left: 0;
      color: #0061bb;
      font-size: 8px;
      position: absolute;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .republic-of-the {
      position: absolute;
      width: 316px;
      top: 52px;
      left: 157px;
      font-family: "Inter-Regular", Helvetica;
      font-weight: 400;
      color: #000000;
      font-size: 12px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-6 {
      position: absolute;
      width: 316px;
      top: 110px;
      left: 157px;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #000000;
      font-size: 20px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
      white-space: nowrap;
    }

    .box .text-wrapper-7 {
      width: 316px;
      top: 168px;
      left: 157px;
      color: #000000;
      font-size: 20px;
      white-space: nowrap;
      position: absolute;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .line {
      position: absolute;
      width: 555px;
      height: 3px;
      top: 146px;
      left: 37px;
    }

    .box .line-2 {
      position: absolute;
      width: 555px;
      height: 1px;
      top: 151px;
      left: 37px;
      object-fit: cover;
    }

    .box .overlap-wrapper {
      position: absolute;
      width: 172px;
      height: 366px;
      top: 210px;
      left: 37px;
    }

    .box .overlap-2 {
      position: relative;
      height: 366px;
    }

    .box .rectangle-2 {
      position: absolute;
      width: 170px;
      height: 363px;
      top: 3px;
      left: 0;
      background-color: #727272;
      border: 1px solid;
      border-color: #000000;
    }

    .box .rectangle-3 {
      position: absolute;
      width: 170px;
      height: 363px;
      top: 0;
      left: 2px;
      background-color: #ffffff;
      border: 1px solid;
      border-color: #000000;
    }

    .box .div-wrapper {
      position: absolute;
      width: 113px;
      height: 24px;
      top: 16px;
      left: 30px;
    }

    .box .group-2 {
      position: absolute;
      width: 113px;
      height: 24px;
      top: 66px;
      left: 29px;
    }

    .box .group-3 {
      position: absolute;
      width: 113px;
      height: 24px;
      top: 331px;
      left: 30px;
    }

    .box .group-4 {
      position: absolute;
      width: 154px;
      height: 25px;
      top: 97px;
      left: 12px;
    }

    .box .text-wrapper-8 {
      position: absolute;
      width: 150px;
      top: 0;
      left: 0;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #000000;
      font-size: 12px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-9 {
      top: 15px;
      left: 18px;
      position: absolute;
      width: 108px;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #0061bb;
      font-size: 8px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .group-5 {
      position: absolute;
      width: 113px;
      height: 24px;
      top: 157px;
      left: 30px;
    }

    .box .group-6 {
      position: absolute;
      width: 137px;
      height: 25px;
      top: 127px;
      left: 20px;
    }

    .box .text-wrapper-10 {
      position: absolute;
      width: 133px;
      top: 0;
      left: 0;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #000000;
      font-size: 12px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-11 {
      top: 15px;
      left: 8px;
      position: absolute;
      width: 108px;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #0061bb;
      font-size: 8px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .group-7 {
      position: absolute;
      width: 113px;
      height: 24px;
      top: 186px;
      left: 30px;
    }

    .box .group-8 {
      position: absolute;
      width: 144px;
      height: 25px;
      top: 217px;
      left: 17px;
    }

    .box .text-wrapper-12 {
      position: absolute;
      width: 140px;
      top: 0;
      left: 0;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #000000;
      font-size: 12px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .text-wrapper-13 {
      top: 15px;
      left: 14px;
      position: absolute;
      width: 108px;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #0061bb;
      font-size: 8px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .group-9 {
      position: absolute;
      width: 113px;
      height: 24px;
      top: 277px;
      left: 31px;
    }

    .box .group-10 {
      position: absolute;
      width: 136px;
      height: 25px;
      top: 303px;
      left: 21px;
    }

    .box .text-wrapper-14 {
      position: absolute;
      width: 132px;
      top: 0;
      left: 0;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #000000;
      font-size: 12px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .group-11 {
      position: absolute;
      width: 131px;
      height: 25px;
      top: 247px;
      left: 21px;
    }

    .box .text-wrapper-15 {
      position: absolute;
      width: 127px;
      top: 0;
      left: 0;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #000000;
      font-size: 12px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .download {
      position: absolute;
      width: 60px;
      height: 60px;
      top: 37px;
      left: 74px;
      object-fit: cover;
    }

    .box .photo {
      position: absolute;
      width: 82px;
      height: 80px;
      top: 208px;
      left: 486px;
    }

    .box .overlap-3 {
      position: relative;
      width: 80px;
      height: 80px;
      background-color: #ffffff;
      border: 1px solid;
      border-color: #000000;
    }

    .box .text-wrapper-16 {
      position: absolute;
      width: 43px;
      height: 17px;
      top: 30px;
      left: 17px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .photo-2 {
      position: absolute;
      width: 171px;
      height: 80px;
      top: 480px;
      left: 433px;
    }

    .box .div-2 {
      position: absolute;
      width: 80px;
      height: 80px;
      top: 0;
      left: 0;
      background-color: #ffffff;
      border: 1px solid;
      border-color: #000000;
    }

    .box .text-wrapper-17 {
      position: absolute;
      width: 70px;
      height: 17px;
      top: 0;
      left: 4px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 8px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .photo-3 {
      position: absolute;
      width: 83px;
      height: 80px;
      top: 0;
      left: 88px;
    }

    .box .overlap-group-2 {
      position: relative;
      width: 81px;
      height: 80px;
    }

    .box .text-wrapper-18 {
      position: absolute;
      width: 81px;
      height: 17px;
      top: 1px;
      left: 0;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 8px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .name {
      position: absolute;
      width: 207px;
      height: 19px;
      top: 237px;
      left: 225px;
    }

    .box .overlap-4 {
      position: relative;
      width: 205px;
      height: 19px;
    }

    .box .line-3 {
      position: absolute;
      width: 205px;
      height: 1px;
      top: 18px;
      left: 0;
      object-fit: cover;
    }

    .box .full-name {
      position: absolute;
      width: 163px;
      height: 19px;
      top: 0;
      left: 21px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
      background: transparent;
      border: none;
      padding: 0;
    }

    .box .age {
      width: 165px;
      top: 262px;
      left: 225px;
      position: absolute;
      height: 19px;
    }

    .box .text-wrapper-19 {
      width: 60px;
      top: 0;
      left: 0;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      letter-spacing: 0;
      line-height: normal;
      position: absolute;
      height: 19px;
    }

    .box .text-wrapper-20 {
      position: absolute;
      width: 8px;
      height: 19px;
      top: 0;
      left: 69px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .overlap-5 {
      position: absolute;
      width: 82px;
      height: 19px;
      top: 0;
      left: 77px;
    }

    .box .text-wrapper-21 {
      position: absolute;
      width: 73px;
      height: 19px;
      top: 0;
      left: 9px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .line-4 {
      position: absolute;
      width: 71px;
      height: 1px;
      top: 16px;
      left: 0;
      object-fit: cover;
    }

    .box .gender {
      width: 165px;
      top: 283px;
      left: 225px;
      position: absolute;
      height: 19px;
    }

    .box .civil-status {
      position: absolute;
      width: 165px;
      height: 19px;
      top: 303px;
      left: 225px;
    }

    .box .address {
      width: 321px;
      top: 324px;
      left: 225px;
      position: absolute;
      height: 19px;
    }

    .box .overlap-6 {
      position: absolute;
      width: 238px;
      height: 19px;
      top: 0;
      left: 77px;
    }

    .box .text-wrapper-22 {
      width: 229px;
      top: 0;
      left: 9px;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      letter-spacing: 0;
      line-height: normal;
      position: absolute;
      height: 19px;
    }

    .box .line-5 {
      position: absolute;
      width: 238px;
      height: 1px;
      top: 16px;
      left: 0;
      object-fit: cover;
    }

    .box .line-6 {
      position: absolute;
      width: 296px;
      height: 1px;
      top: 473px;
      left: 225px;
      object-fit: cover;
    }

    .box .download-2 {
      position: absolute;
      width: 60px;
      height: 60px;
      top: 37px;
      left: 496px;
      object-fit: cover;
    }

    .box .line-7 {
      position: absolute;
      width: 122px;
      height: 1px;
      top: 507px;
      left: 297px;
      object-fit: cover;
    }

    .box .group-12 {
      position: absolute;
      width: 202px;
      height: 15px;
      top: 548px;
      left: 223px;
    }

    .box .overlap-7 {
      position: relative;
      width: 200px;
      height: 15px;
    }

    .box .text-wrapper-23 {
      position: absolute;
      width: 198px;
      height: 15px;
      top: 0;
      left: 0;
      font-family: "Inter-Medium", Helvetica;
      font-weight: 500;
      color: #000000;
      font-size: 11px;
      text-align: center;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .line-8 {
      position: absolute;
      width: 198px;
      height: 1px;
      top: 0;
      left: 2px;
      object-fit: cover;
    }
  </style>
  <div class="group">
    <div class="overlap">
      <div class="rectangle"></div>
      <img class="screenshot" src={footer} />
      <img class="img" src={clerk} />
      <p class="text-wrapper">San Nicolas III Bacoor City, Cavite</p>
      <p class="div">
        According to the records kept in this office, the subject has no pending
        cases. The resident has known to be a GOOD MORAL CHARACTER and
        LAW-ABIDING CITIZEN of this community
      </p>
      <p class="p">This Barangay Clearance is issued upon for</p>
      <p class="given-this-day-jan">Given this day,&nbsp;&nbsp;{convertDateFormat(dateOfAppointment)}</p>
      <div class="text-wrapper-2">Prepared By:</div>
      <div class="text-wrapper-3">{purpose}</div>
      <div class="overlap-group-wrapper">
        <div class="overlap-group">
          <div class="text-wrapper-4">Alfredo F.&nbsp;&nbsp;Arevalo</div>
          <div class="text-wrapper-5">Barangay Chairman</div>
        </div>
      </div>
      <p class="republic-of-the">
        Republic of the Philippines<br />Province of Cavite<br />City of BACOOR
      </p>
      <div class="text-wrapper-6">BARANGAY SAN NICOLAS III</div>
      <div class="text-wrapper-7">BARANGAY CLEARANCE</div>
      <img class="line" src="img/line-1.svg" />
      <svg
        class="line"
        width="555"
        height="3"
        viewBox="0 0 555 3"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <line y1="1.5" x2="555.001" y2="1.5" stroke="black" stroke-width="3" />
      </svg>

      <svg
        class="line-2"
        width="555"
        height="1"
        viewBox="0 0 555 1"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <line y1="0.5" x2="555.001" y2="0.5" stroke="black" />
      </svg>

      <div class="overlap-wrapper">
        <div class="overlap-2">
          <div class="rectangle-2"></div>
          <div class="rectangle-3"></div>
          <div class="div-wrapper">
            <div class="overlap-group">
              <div class="text-wrapper-4">Alfredo F.&nbsp;&nbsp;Arevalo</div>
              <div class="text-wrapper-5">Barangay Chairman</div>
            </div>
          </div>
          <div class="group-2">
            <div class="overlap-group">
              <div class="text-wrapper-4">Manuel F. Arevalo</div>
              <div class="text-wrapper-5">Councilor</div>
            </div>
          </div>
          <div class="group-3">
            <div class="overlap-group">
              <div class="text-wrapper-4">Lyziel S. Gonzales</div>
              <div class="text-wrapper-5">Barangay Treasurer</div>
            </div>
          </div>
          <div class="group-4">
            <div class="text-wrapper-8">Domingo D. Jimenez Jr.</div>
            <div class="text-wrapper-9">Councilor</div>
          </div>
          <div class="group-5">
            <div class="overlap-group">
              <div class="text-wrapper-4">Leona V. Frias</div>
              <div class="text-wrapper-5">Councilor</div>
            </div>
          </div>
          <div class="group-6">
            <div class="text-wrapper-10">George L. Gawaran Jr</div>
            <div class="text-wrapper-11">Councilor</div>
          </div>
          <div class="group-7">
            <div class="overlap-group">
              <div class="text-wrapper-4">Juddy E. Better</div>
              <div class="text-wrapper-5">Councilor</div>
            </div>
          </div>
          <div class="group-8">
            <div class="text-wrapper-12">Jeric C. Delos Santos</div>
            <div class="text-wrapper-13">Councilor</div>
          </div>
          <div class="group-9">
            <div class="overlap-group">
              <div class="text-wrapper-4">Niña L. Cañete</div>
              <div class="text-wrapper-5">S.K. Chairman</div>
            </div>
          </div>
          <div class="group-10">
            <div class="text-wrapper-14">Ricelyn A. Custodio</div>
            <div class="text-wrapper-11">Barangay Secretary</div>
          </div>
          <div class="group-11">
            <div class="text-wrapper-15">Nelson S. Campasivo</div>
            <div class="text-wrapper-11">Barangay Chairman</div>
          </div>
        </div>
      </div>
      <img class="download" src={nicolasLogo} />
      <div class="photo">
        <div class="overlap-3"><div class="text-wrapper-16">Photo</div></div>
      </div>
      <div class="photo-2">
        <div class="div-2">
          <div class="text-wrapper-17">Left Thumb Mark</div>
        </div>
        <div class="photo-3">
          <div class="overlap-group-2">
            <div class="div-2"></div>
            <div class="text-wrapper-18">Right Thumb Mark</div>
          </div>
        </div>
      </div>
      <div class="name">
        <div class="overlap-4">
          <svg
            class="line-3"
            width="205"
            height="1"
            viewBox="0 0 205 1"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <line y1="0.5" x2="205" y2="0.5" stroke="black" />
          </svg>

          <input
            class="full-name"
            placeholder="Full Name"
            type="text"
            bind:value={completeName}
          />
        </div>
      </div>
      <div class="age">
        <div class="text-wrapper-19">Age</div>
        <div class="text-wrapper-20">:</div>
        <div class="overlap-5">
          <div class="text-wrapper-21">{age}</div>
          <img class="line-4" src="img/image.svg" />
        </div>
      </div>
      <div class="gender">
        <div class="text-wrapper-19">Gender</div>
        <div class="text-wrapper-20">:</div>
        <div class="overlap-5">
          <div class="text-wrapper-21">{gender}</div>
          <img class="line-4" src="img/line-4-2.svg" />
        </div>
      </div>
      <div class="civil-status">
        <div class="text-wrapper-19">Civil Status</div>
        <div class="text-wrapper-20">:</div>
        <div class="overlap-5">
          <div class="text-wrapper-21">{civilStatus}</div>
          <img class="line-4" src="img/line-4-3.svg" />
        </div>
      </div>
      <div class="address">
        <div class="text-wrapper-19">Address</div>
        <div class="text-wrapper-20">:</div>
        <div class="overlap-6">
          <div class="text-wrapper-22">{address}</div>
          <img class="line-5" src="img/line-4.svg" />
        </div>
      </div>
      <svg
        class="line-6"
        width="296"
        height="1"
        viewBox="0 0 296 1"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <line y1="0.5" x2="296" y2="0.5" stroke="black" />
      </svg>

      <img class="download-2" src={bacoorLogo} />
      <svg
        class="line-7"
        width="122"
        height="1"
        viewBox="0 0 122 1"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <line y1="0.5" x2="122" y2="0.5" stroke="black" />
      </svg>

      <div class="group-12">
        <div class="overlap-7">
          <div class="text-wrapper-23">Signature over printed name</div>
          <svg
            class="line-8"
            width="198"
            height="1"
            viewBox="0 0 198 1"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <line y1="0.5" x2="198" y2="0.5" stroke="black" />
          </svg>
        </div>
      </div>
    </div>
  </div>
</div>
