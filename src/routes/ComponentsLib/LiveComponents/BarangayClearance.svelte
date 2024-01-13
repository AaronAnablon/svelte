<script>
  import Button from "../GeneralComponents/Button.svelte";
  import Inputs from "../GeneralComponents/Inputs.svelte";
  import { fly } from "svelte/transition";

  import {
    onSnapsClearance,
    showClearanceAddModal,
    compareClearanceValue,
    printing,
  } from "../BoundComponents/clickOutside";
  import { showPrintModel, formattedDate } from "./stateStore";

  import bgyClearance from "../Images/bgyClearance.jpg";
  import PrintContent from "./PrintContent.svelte";

  //database calls and hooks
  import { auth, db } from "../../db/firebase";
  import {
    onSnapshot,
    addDoc,
    collection,
    serverTimestamp,
    increment,
    doc,
    deleteDoc,
    query,
    orderBy,
    setDoc,
    where,
  } from "firebase/firestore";

  //handler to show add modal
  const toShowAddModal = () => {
    showClearanceAddModal.set(true);
  };

  //barangayID varStore
  const bgyVarStore = {
    firstName: "",
    lastName: "",
    middleInitial: "",
    suffixName: "",
    address: "",
    age: "",
    lengthOfStay: "",
    purpose: "",
    dateOfAppointment: "",
    kwiri: "",
    trigger: false,
  };

  import reportHeader from "./images/header_report.png";

  //submit data to database
  const submitData = async () => {
    const colRef = collection(db, "barangayClearance");
    addDoc(colRef, {
      createdAt: serverTimestamp(),
      bgyClearanceCounter: increment(1),
      firstName: bgyVarStore.firstName.BINDTHIS,
      lastName: bgyVarStore.lastName.BINDTHIS,
      middleInitial: bgyVarStore.middleInitial.BINDTHIS,
      suffixName: bgyVarStore.suffixName.BINDTHIS,
      address: bgyVarStore.address.BINDTHIS,
      age: bgyVarStore.age.BINDTHIS,
      lengthOfStay: bgyVarStore.lengthOfStay.BINDTHIS,
      purpose: bgyVarStore.purpose.BINDTHIS,
      dateOfAppointment: bgyVarStore.dateOfAppointment.BINDTHIS,
    }).then(() => {
      showClearanceAddModal.set(false);
    });
  };

  //fetch data from database
  const colRef = collection(db, "barangayClearance");
  const q = query(colRef, orderBy("createdAt", "desc"));
  onSnapshot(q, (snapshots) => {
    let fbData = [];
    snapshots.docs.forEach((doc) => {
      let data = { ...doc.data(), id: doc.id };
      fbData = [data, ...fbData];
    });
    onSnapsClearance.set(fbData);
  });

  //removeData from database
  const removeData = async (data) => {
    const docRef = doc(colRef, data);
    await deleteDoc(docRef);
  };

  const updateStatus = async (userID, selectedStatus) => {
    const docRef = doc(colRef, userID);

    const updatedData = {
      status: selectedStatus,
      lastUpdated: serverTimestamp(),
    };

    await setDoc(docRef, updatedData, { merge: true });
  };

  //updateData from database
  const updateData = async (data) => {
    const docRef = doc(colRef, data);
    setDoc(
      docRef,
      {
        lastUpdated: serverTimestamp(),
        completeName: bgyVarStore.completeName.BINDTHIS,
        address: bgyVarStore.address.BINDTHIS,
        birthdate: bgyVarStore.birthdate.BINDTHIS,
        lengthOfStay: bgyVarStore.lengthOfStay.BINDTHIS,
        purpose: bgyVarStore.purpose.BINDTHIS,
        dateOfAppointment: bgyVarStore.dateOfAppointment.BINDTHIS,
      },
      { merge: true }
    );
  };

  //showModalComparison
  const editValueHandler = (data) => {
    compareClearanceValue.set(data);
  };

  const handlerSearch = () => {
    if (bgyVarStore.trigger) {
      const q = query(
        colRef,
        orderBy("createdAt", "desc"),
        where("completeName", "==", bgyVarStore.kwiri)
      );
      onSnapshot(q, (snapshots) => {
        let fbData = [];
        snapshots.docs.forEach((doc) => {
          let data = { ...doc.data(), id: doc.id };
          fbData = [data, ...fbData];
        });
        onSnapsClearance.set(fbData);
      });

      bgyVarStore.trigger = false;
    }
  };

  const detectInputs = () => {
    if (bgyVarStore.kwiri.trim().length < 1) {
      const q = query(colRef, orderBy("createdAt", "desc"));
      onSnapshot(q, (snapshots) => {
        let fbData = [];
        snapshots.docs.forEach((doc) => {
          let data = { ...doc.data(), id: doc.id };
          fbData = [data, ...fbData];
        });
        onSnapsClearance.set(fbData);
      });
      bgyVarStore.trigger = false;
    } else {
      bgyVarStore.trigger = true;
    }
  };

  async function printPdf(dataForPrint) {
    // console.log({ dataForPrint });
    // printData.name = dataForPrint.completeName;
    // printData.activity = dataForPrint.purpose;
    // pdfElementHidden = false;
    // const pdf = await html2pdf().set(opt).from(pdfElement).save();
    // pdfElementHidden = true;
    // return pdf;

    printPdfTest(dataForPrint);

    return true;
  }
  function printPdfTest(dataForPrint) {
    const mywindow = window.open(
      "",
      "PRINT",
      "height=891,width=649, initial-scale=1.0"
    );
    console.log(dataForPrint);

    if (!mywindow) {
      console.error("Popup blocked. Please allow popups for this site.");
      return false;
    }

    const printContent = new PrintContent({
      target: mywindow.document.body,
      props: {
        documentTitle: "hello world",
        mywindow: mywindow,
        ...dataForPrint,
      },
    });
    console.log(printContent);

    // mywindow.document.close(); // Necessary for IE >= 10
    // mywindow.focus(); // Necessary for IE >= 10

    mywindow.addEventListener("mousedown", function () {
      // Code to execute after the print operation is complete
      console.log("Print operation completed");
      // Optionally, close the window after printing
      // mywindow.print();
      // mywindow.close();
    });

    // mywindow.close();

    return true;
  }
</script>

<div class="m-2 mx-auto text-xs">
  <div class="min-h-[50vh] p-10">
    <div class="flex gap-2 items-center mb-2">
      <div class="w-full flex gap-2">
        <div class="">
          <Button TITLE="Add Barangay Clearance" on:click={toShowAddModal} />
        </div>

        <div class="">
          <Button
            TITLE="Generate Barangay Clearance"
            on:click={() => showPrintModel.set(true)}
          />
        </div>
      </div>

      <div class="flex flex-row-reverse items-center w-full">
        <button
          class="bg-blue-400 text-white absolute p-2 border-r-2 border-black hover:bg-blue-700 font-bold"
          on:click={handlerSearch}>Search</button
        >
        <input
          type="text"
          placeholder="Complete Name Only"
          class="w-[40%] p-2 focus:outline-none border-2 border-black"
          on:keyup={detectInputs}
          bind:value={bgyVarStore.kwiri}
        />
      </div>

      {#if $showPrintModel}
        <div class="fixed bottom-0 top-0 left-0 right-0 bg-white">
          <div class="mx-auto max-w-[1000px] mt-[20vh] p-10">
            <div class="fixed bottom-0 right-0 p-10">
              <div class="flex gap-2">
                {#if !$printing}
                  <div class="">
                    <Button
                      TITLE="Print Now"
                      on:click={() => {
                        $printing = true;
                        // print();
                        // $printing = false;
                        setTimeout(() => print(), 100);
                        setTimeout(() => {
                          $printing = false;
                          $showPrintModel = false;
                        }, 2000);
                      }}
                    />
                  </div>
                  <div class="">
                    <Button
                      TITLE="Close"
                      on:click={() => showPrintModel.set(false)}
                    />
                  </div>
                {/if}
              </div>
            </div>
          </div>
        </div>
      {/if}
    </div>

    {#if $showClearanceAddModal}
      <div
        class="flex flex-col gap-2 bg-white p-4 max-w-fit mx-auto rounded-lg mt-2 absolute left-0 right-0 border-2 border-guiColor z-10"
      >
        <p class="text-xl text-center font-bold p-2 text-slate-500">
          New Barangay Clearance
        </p>
        <div class="flex justify-center gap-2">
          <div class="">
            <Inputs
              TITLE="First Name"
              PLACEHOLDER="First Name"
              bind:this={bgyVarStore.firstName}
            />
          </div>
          <div class="">
            <Inputs
              TITLE="M.I."
              PLACEHOLDER="M.I."
              bind:this={bgyVarStore.middleInitial}
            />
          </div>
        </div>
        <div class="flex justify-center gap-2">
          <div class="">
            <Inputs
              TITLE="Last Name"
              PLACEHOLDER="Last Name"
              bind:this={bgyVarStore.lastName}
            />
          </div>
          <div class="">
            <Inputs
              TITLE="Suffix"
              PLACEHOLDER="Suffix"
              bind:this={bgyVarStore.suffixName}
            />
          </div>
        </div>

        <div class="flex justify-center gap-2">
          <div class="">
            <Inputs
              TITLE="Length of stay"
              PLACEHOLDER="Length of stay"
              bind:this={bgyVarStore.lengthOfStay}
            />
          </div>
          <div class="">
            <Inputs
              TITLE="Purpose"
              PLACEHOLDER="purpose"
              bind:this={bgyVarStore.purpose}
            />
          </div>
        </div>

        <div class="flex justify-center gap-2">
          <div class="w-full">
            <Inputs
              TITLE="Age"
              PLACEHOLDER="18"
              TYPE="number"
              bind:this={bgyVarStore.age}
            />
          </div>
        </div>

        <div class="">
          <Inputs
            TITLE="Complete Address:"
            PLACEHOLDER="Complete Address"
            bind:this={bgyVarStore.address}
          />
        </div>

        <div class="">
          <Inputs
            TITLE="Date Of Appointment:"
            TYPE="date"
            PLACEHOLDER=""
            bind:this={bgyVarStore.dateOfAppointment}
          />
        </div>

        <div class="flex gap-2">
          <Button TITLE="Submit" on:click={submitData} />
          <Button
            TITLE="Cancel"
            on:click={() => {
              showClearanceAddModal.set(false);
            }}
          />
        </div>
      </div>
    {/if}
    <div class="" in:fly={{ x: -400, duration: 1000 }}>
      <div class="relative">
        {#if $showPrintModel}
          <img src={reportHeader} alt="" style="margin-top: -130px; margin-bottom: 100px" />
        {/if}
        <table class="w-full text-sm text-left text-gray-500">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50">
            <tr>
              <th scope="col" class="px-6 py-3"> firstname </th>
              <th scope="col" class="px-6 py-3"> MI </th>
              <th scope="col" class="px-6 py-3"> lastname </th>
              <th scope="col" class="px-6 py-3"> suffix </th>
              <th scope="col" class="px-6 py-3"> address </th>
              <th scope="col" class="px-6 py-3"> age </th>
              <th scope="col" class="px-6 py-3"> length of stay </th>
              <th scope="col" class="px-6 py-3"> purpose </th>
              <th scope="col" class="px-6 py-3"> date of appointment </th>
              <th scope="col" class="px-6 py-3"> status </th>
              {#if !$showPrintModel}
              <th scope="col" class="px-6 py-3"> action </th>
              {/if}
            </tr>
          </thead>
          <tbody>
            {#each $onSnapsClearance as barangayClearance, i}
              <!-- {console.log(barangayClearance)} -->
              <tr class="bg-white border-b">
                <th
                  scope="row"
                  class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap"
                >
                  {barangayClearance.firstName}
                </th>
                <td class="px-6 py-4">
                  {barangayClearance.middleInitial}
                </td>
                <td class="px-6 py-4">
                  {barangayClearance.lastName}
                </td>
                <td class="px-6 py-4">
                  {barangayClearance.suffixName}
                </td>
                <td class="px-6 py-4">
                  {barangayClearance.address}
                </td>
                <td class="px-6 py-4">
                  {barangayClearance.age}
                </td>
                <td class="px-6 py-4">
                  {barangayClearance.lengthOfStay}
                </td>
                <td class="px-6 py-4">
                  {barangayClearance.purpose}
                </td>
                <td class="px-6 py-4">
                  {barangayClearance.dateOfAppointment}
                </td>
                {#if $showPrintModel}

                <td class="px-6 py-4">
                  {barangayClearance.status}
                </td>
                {/if}
                {#if !$showPrintModel}
                <td class="px-6 py-4">
                  <select
                    class="bg-white"
                    bind:value={barangayClearance.status}
                    on:change={() =>
                      updateStatus(
                        barangayClearance.id,
                        barangayClearance.status
                      )}
                  >
                    <option value="Processing">On Process</option>
                    <option value="Ready for pickup">For Pickup</option>
                    <option value="Claimed">Completed</option>
                  </select>
                </td>
                <td>
                  <div class="flex gap-2">
                    <button
                      class="hover:bg-orange-300 rounded-full px-4 py-2 hover:scale-105 duration-700"
                      on:click={() => {
                        editValueHandler(i);
                      }}><i class="ri-pencil-line"></i></button
                    >

                    <button
                      class="hover:bg-red-500 rounded-full px-4 py-2 hover:scale-105 duration-700 text-red-900 hover:text-white"
                      on:click={removeData(barangayClearance.id)}
                      ><i class="ri-delete-bin-line"></i></button
                    >

                    <button
                      class="hover:bg-blue-500 rounded-full px-4 py-2 hover:scale-105 duration-700 text-blue-900 hover:text-white"
                      on:click={() => printPdf(barangayClearance)}
                      ><i class="ri-printer-line"></i></button
                    >
                  </div>
                </td>
                {/if}
              </tr>
              {#if $compareClearanceValue === i}
                <div class="">
                  <div
                    class="flex flex-col gap-2 p-4 max-w-fit mx-auto rounded-lg mt-2 absolute left-0 right-0 border-2 border-slate-200 z-10"
                  >
                    <p class="text-xl text-center font-bold p-2 text-slate-500">
                      Modify Values
                    </p>
                    <div class="">
                      <Inputs
                        TITLE="Complete Name:"
                        PLACEHOLDER="Complete Name"
                        bind:this={bgyVarStore.completeName}
                      />
                    </div>

                    <div class="flex justify-center gap-2">
                      <div class="">
                        <Inputs
                          TITLE="Length of stay"
                          PLACEHOLDER="Length of stay"
                          bind:this={bgyVarStore.lengthOfStay}
                        />
                      </div>
                      <div class="">
                        <Inputs
                          TITLE="Purpose"
                          PLACEHOLDER="purpose"
                          bind:this={bgyVarStore.purpose}
                        />
                      </div>
                    </div>

                    <div class="flex justify-center gap-2">
                      <div class="w-full">
                        <Inputs
                          TITLE="Birthdate:"
                          TYPE="date"
                          bind:this={bgyVarStore.birthdate}
                        />
                      </div>
                    </div>

                    <div class="">
                      <Inputs
                        TITLE="Complete Address:"
                        PLACEHOLDER="Complete Address"
                        bind:this={bgyVarStore.address}
                      />
                    </div>

                    <div class="">
                      <Inputs
                        TITLE="Date Of Appointment:"
                        TYPE="date"
                        PLACEHOLDER=""
                        bind:this={bgyVarStore.dateOfAppointment}
                      />
                    </div>

                    <div class="flex gap-2">
                      <button
                        class="bg-orange-300 px-4 py-2 rounded-lg w-1/2"
                        on:click={updateData(barangayClearance.id)}
                      >
                        Update
                      </button>
                      <button
                        class="bg-red-300 px-4 py-2 rounded-lg w-1/2"
                        on:click={() => {
                          compareClearanceValue.set("");
                        }}
                      >
                        Cancel
                      </button>
                    </div>
                  </div>
                </div>
              {/if}
            {/each}
          </tbody>
        </table>
      </div>

      <!-- {#each $onSnapsClearance as value, i}
            <div class="flex justify-center items-center " in:fly={{x:-400, duration:1000}}>
                <p class="w-[25%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Complete Name</p>
                <p class="w-full border-b-2 border-white bg-slate-100 p-2 ">{value.completeName}</p>

                <p class="w-[10%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Address</p>
                <p class="w-full border-b-2 border-white bg-slate-100 p-2 ">{value.address}</p>

                <p class="w-[17%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Birth date</p>
                <p class="w-[20%] border-2 border-white bg-slate-100 p-2 ">{value.birthdate}</p>

                <p class="w-[25%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Length Of Stay</p>
                <p class="w-[20%] border-2 border-white bg-slate-100 p-2 ">{value.lengthOfStay}</p>

                <p class="w-[12%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Purpose</p>
                <p class="w-[20%] border-2 border-white bg-slate-100 p-2 ">{value.purpose}</p>

                <p class="w-[33%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Date Of Appointment</p>
                <p class="w-[20%] border-2 border-white bg-slate-100 p-2 ">{value.dateOfAppointment}</p>

                <div class="flex w-[30%] bg-slate-100">
                    <button class="bg-red-500 font-bold text-white w-full p-2 hover:bg-red-600 border-b-2 border-white"
                    on:click={removeData(value.id)}
                    >Delete</button>

                    <button class="bg-blue-500 font-bold text-white w-full p-2 hover:bg-blue-600 border-b-2 border-white"
                    on:click={()=>{editValueHandler(i)}}
                    >Update</button>

                    <button class="bg-red-500 font-bold text-white w-full p-2 hover:bg-red-600 border-b-2 border-white"
                    on:click={() => showPrintModel.set(true)}
                    >Print</button>
                    
                </div>
                {#if $compareClearanceValue === i}
                    <div class="">
                        <div class="flex flex-col gap-2 bg-guiColor p-4 max-w-fit mx-auto rounded-lg mt-2 absolute left-0 right-0 border-2 border-slate-200 z-10">
                            <p class="text-xl text-center font-bold p-2 text-slate-500">Modify Values</p>
                            <div class="">
                                <Inputs TITLE="Complete Name:" PLACEHOLDER="Complete Name" bind:this={bgyVarStore.completeName}/>
                            </div>
                
                            <div class="flex justify-center gap-2">
                                <div class="">
                                    <Inputs TITLE="Length of stay" PLACEHOLDER="Length of stay" bind:this={bgyVarStore.lengthOfStay}/>
                                </div>
                                <div class="">
                                    <Inputs TITLE="Purpose" PLACEHOLDER="purpose" bind:this={bgyVarStore.purpose}/>
                                </div>
                            </div>
                
                            <div class="flex justify-center gap-2">
                                <div class="w-full">
                                    <Inputs TITLE="Birthdate:" TYPE="date" bind:this={bgyVarStore.birthdate}/>
                                </div>
                            
                            </div>
                
                            <div class="">
                                <Inputs TITLE="Complete Address:" PLACEHOLDER="Complete Address" bind:this={bgyVarStore.address}/>
                            </div>
                            
                            <div class="">
                                <Inputs TITLE="Date Of Appointment:" TYPE="date" PLACEHOLDER="" bind:this={bgyVarStore.dateOfAppointment}/>
                            </div>

                            <div class="flex gap-2">
                                <Button TITLE="Confirm Edit" on:click={updateData(value.id)}/>
                                <Button TITLE="Cancel" on:click={() => {compareClearanceValue.set("")}}/>
                            </div>
                        </div>
                    </div>
                {/if}
            </div>
            {/each} -->
    </div>
  </div>
</div>
