<!-- CertificateContent.svelte -->
<script>
  export let documentTitle = "";
  export let completeName = "";
  export let age = "";
  export let purpose = "";
  export let dateOfAppointment = "";
  export let mywindow = null;

  import header from "./images/certificateheader.png";
  import footer from "./images/certificateFooter.png";

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
      position: relative;
      width: 631px;
      height: 891px;
    }

    .box .certificate {
      position: fixed;
      width: 631px;
      height: 891px;
      top: 0;
      left: 0;
      background-color: #ffffff;
    }

    .box .header {
      position: absolute;
      width: 611px;
      height: 176px;
      top: 0;
      left: 10px;
      object-fit: cover;
    }

    .box .content {
      position: absolute;
      width: 554px;
      height: 228px;
      top: 200px;
      left: 26px;
    }

    .box .overlap-group {
      position: relative;
      width: 550px;
      height: 228px;
    }

    .box .text-wrapper {
      position: absolute;
      width: 204px;
      top: 0;
      left: 0;
      font-family: "Inter-Bold", Helvetica;
      font-weight: 700;
      color: #000000;
      font-size: 13px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .this-is-to-certify {
      position: absolute;
      width: 536px;
      top: 32px;
      left: 14px;
      font-family: "Inter-Regular", Helvetica;
      font-weight: 400;
      color: #000000;
      font-size: 12px;
      letter-spacing: 0;
      line-height: normal;
    }

    .box .span {
      font-family: "Inter-Regular", Helvetica;
      font-weight: 400;
      color: #000000;
      font-size: 12px;
      letter-spacing: 0;
    }

    .box .text-wrapper-2 {
      text-decoration: underline;
    }

    .box .overlap {
      position: absolute;
      width: 622px;
      height: 452px;
      top: 438px;
      left: 8px;
      background-image: url(./img/footer.png);
      background-size: cover;
      background-position: 50% 50%;
    }

    .box .rectangle {
      top: 98px;
      left: -5px;
      position: absolute;
      width: 18px;
      height: 338px;
      background-color: #ffffff;
    }

    .box .div {
      top: 114px;
      left: 595px;
      position: absolute;
      width: 18px;
      height: 338px;
      background-color: #ffffff;
    }

    .box .footer {
      width: 611px;
      height: 436px;
      transform: rotate(1.5deg);
      flex-shrink: 0;
    }
  </style>
  <div class="certificate">
    <img class="header" src={header} />
    <div class="content">
      <div class="overlap-group">
        <p class="text-wrapper">TO WHOM IT MAY CONCERN:</p>
        <p class="this-is-to-certify">
          <span class="span"
            >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This
            is to certify that
          </span>
          <span class="text-wrapper-2"
            >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            {completeName}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
          </span>
          <span class="span">,<br /></span>
          <span class="text-wrapper-2">&nbsp;&nbsp; {age} &nbsp;&nbsp;</span>
          <span class="span">
            years of age, Filipino, appeared here in this office of Barangay
            Hall San Nicolas III, City of Bacoor Cavite and conducted
            <span class="text-wrapper-2"
              >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{purpose}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span
            >
            <br /><br /><br
            />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;This
            certificate of appearance is being issued upon request of above
            mentioned name<br />for whatever legal purposes it may serve.
            <br /><br /><br
            />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Issued
            this
            <span class="text-wrapper-2"
              >&nbsp;&nbsp;{convertDateFormat(
                dateOfAppointment
              )[1]}{getOrdinalSuffix(
                convertDateFormat(dateOfAppointment)[1]
              )}&nbsp;&nbsp;</span
            >
            day of
            <span class="text-wrapper-2"
              >&nbsp;&nbsp;{convertDateFormat(
                dateOfAppointment
              )[0]}&nbsp;&nbsp;</span
            >
            {convertDateFormat(dateOfAppointment)[2]} at the office of Barangay Hall
            San Nicolas III City of Bacoor, Cavite.</span
          >
        </p>
      </div>
    </div>
    <div class="overlap">
      <div class="image"><img class="footer" src={footer} /></div>
      <div class="rectangle"></div>
      <div class="div"></div>
    </div>
  </div>
</div>
