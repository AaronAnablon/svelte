<script>
  import Button from "../GeneralComponents/Button.svelte";
  import Inputs from "../GeneralComponents/Inputs.svelte";
  import { fly } from "svelte/transition";
  import {
    onSnapsBgyID,
    compareIDvalue,
    showAddModal,
    printing
  } from "../BoundComponents/clickOutside";
  import { showPrintModel, formattedDate } from "./stateStore";
  import bgyClearance from "../Images/bgyClearance.jpg";

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

  import IdContent from "./IDContent.svelte";
  import reportHeader from "./images/header_report.png";

  //handler to show add modal
  const toShowAddModal = () => {
    showAddModal.set(true);
  };

  //barangayID varStore
  const bgyVarStore = {
    status: "",
    completeName: "",
    address: "",
    birthdate: "",
    gender: "",
    height: "",
    weight: "",
    dateOfAppointment: "",
    kwiri: "",
    trigger: false,
  };

  //submit data to database
  const submitData = async () => {
    const colRef = collection(db, "barangayID");
    addDoc(colRef, {
      createdAt: serverTimestamp(),
      completeName: bgyVarStore.completeName.BINDTHIS,
      address: bgyVarStore.address.BINDTHIS,
      birthdate: bgyVarStore.birthdate.BINDTHIS,
      gender: bgyVarStore.gender.BINDTHIS,
      height: bgyVarStore.height.BINDTHIS,
      weight: bgyVarStore.weight.BINDTHIS,
      dateOfAppointment: bgyVarStore.dateOfAppointment.BINDTHIS,
      barangayCounter: increment(1),
    }).then(() => {
      showAddModal.set(false);
    });
  };

  //fetch data from database
  const colRef = collection(db, "barangayID");
  const q = query(colRef, orderBy("createdAt", "desc"));
  onSnapshot(q, (snapshots) => {
    let fbData = [];
    snapshots.docs.forEach((doc) => {
      let data = { ...doc.data(), id: doc.id };
      fbData = [data, ...fbData];
    });
    onSnapsBgyID.set(fbData);
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
        gender: bgyVarStore.gender.BINDTHIS,
        height: bgyVarStore.height.BINDTHIS,
        weight: bgyVarStore.weight.BINDTHIS,
        dateOfAppointment: bgyVarStore.dateOfAppointment.BINDTHIS,
        status: bgyVarStore.status.BINDTHIS,
      },
      { merge: true }
    );
  };

  //showModalComparison
  const editValueHandler = (data) => {
    compareIDvalue.set(data);
  };

  const handleSearch = () => {
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
      onSnapsBgyID.set(fbData);
    });

    bgyVarStore.trigger = false;
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
        onSnapsBgyID.set(fbData);
      });

      bgyVarStore.trigger = false;
    } else {
      bgyVarStore.trigger = true;
    }
  };

  async function printPdf(dataForPrint) {
    const mywindow = window.open(
      "",
      "PRINT",
      "height=891,width=649, initial-scale=1.0"
    );

    if (!mywindow) {
      console.error("Popup blocked. Please allow popups for this site.");
      return false;
    }
    const printContent = new IdContent({
      target: mywindow.document.body,
      props: {
        documentTitle: "hello world",
        mywindow: mywindow,
        ...dataForPrint,
      },
    });
    console.log(printContent);
    return true;
  }
</script>

<div class="m-2 mx-auto text-xs">
  <div class="min-h-[50vh] p-10">
    <div class=" flex gap-2 items-center mb-2">
      <div class="w-full flex gap-2">
        <div class="">
          <Button TITLE="Add Barangay ID" on:click={toShowAddModal} />
        </div>

        <div class="">
          <Button
            TITLE="Generate Barangay ID"
            on:click={() => showPrintModel.set(true)}
          />
        </div>
      </div>

      <div class="flex flex-row-reverse items-center w-full">
        <button
          class="bg-blue-400 text-white absolute p-2 border-r-2 border-black hover:bg-blue-700 font-bold"
          on:click={handleSearch}>Search</button
        >
        <input
          type="text"
          placeholder="Complete Name Only"
          class="w-[40%] p-2 focus:outline-none border-2 border-black"
          on:keyup={detectInputs}
          bind:value={bgyVarStore.kwiri}
        />
      </div>
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

    {#if $showAddModal}
      <div
        class="flex flex-col gap-2 bg-white p-4 max-w-fit mx-auto rounded-lg mt-2 absolute left-0 right-0 border-2 border-guiColor z-10"
      >
        <p class="text-xl text-center font-bold p-2 text-slate-500">
          New Barangay ID
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
              TITLE="Height"
              PLACEHOLDER="Height"
              bind:this={bgyVarStore.height}
            />
          </div>
          <div class="">
            <Inputs
              TITLE="Weight"
              PLACEHOLDER="Weight"
              bind:this={bgyVarStore.weight}
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
          <div class="w-full">
            <Inputs
              TITLE="Gender:"
              TYPE="cordion"
              bind:this={bgyVarStore.gender}
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
            TITLE="Date Of Apointment:"
            TYPE="date"
            PLACEHOLDER="Complete Address"
            bind:this={bgyVarStore.dateOfAppointment}
          />
        </div>

        <div class="flex gap-2">
          <Button TITLE="Submit" on:click={submitData} />
          <Button
            TITLE="Cancel"
            on:click={() => {
              showAddModal.set(false);
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
              {#if !$showPrintModel}
              <th scope="col" class="px-6 py-3"> ID Image </th>
              {/if}
              <th scope="col" class="px-6 py-3"> Firstname </th>
              <th scope="col" class="px-6 py-3"> MI</th>
              <th scope="col" class="px-6 py-3"> Lastname </th>
              <th scope="col" class="px-6 py-3"> Suffix </th>
              <th scope="col" class="px-6 py-3"> address </th>
              <th scope="col" class="px-6 py-3"> birthdate </th>
              <th scope="col" class="px-6 py-3"> gender </th>
              <th scope="col" class="px-6 py-3"> height </th>
              <th scope="col" class="px-6 py-3"> weight </th>
              <th scope="col" class="px-6 py-3"> Appointment </th>
              <th scope="col" class="px-6 py-3"> status </th>
              {#if !$showPrintModel}
              <th scope="col" class="px-6 py-3"> action </th>
              {/if}
            </tr>
          </thead>
          <tbody>
            {#each $onSnapsBgyID as barangayId, i}
              <tr class="bg-white border-b">
              {#if !$showPrintModel}

                <th
                  scope="row"
                  class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap"
                >
                  <img
                    src={barangayId.IDpictureUrl}
                    alt="ID Picture"
                    class="h-10 w-10 rounded-full"
                  />
                </th>
                {/if}
                <td class="px-6 py-4">
                  {barangayId.firstName}
                </td>
                <td class="px-6 py-4">
                  {barangayId.lastName}
                </td>
                <td class="px-6 py-4">
                  {barangayId.middleInitial}
                </td>
                <td class="px-6 py-4">
                  {barangayId.suffixName}
                </td>
                <td class="px-6 py-4">
                  {barangayId.address}
                </td>
                <td class="px-6 py-4">
                  {barangayId.birthDate}
                </td>
                <td class="px-6 py-4">
                  {barangayId.gender}
                </td>
                <td class="px-6 py-4">
                  {barangayId.height}
                </td>
                <td class="px-6 py-4">
                  {barangayId.weight}
                </td>
                <td class="px-6 py-4">
                  {barangayId.dateOfAppointment}
                </td>
              {#if $showPrintModel}
              <td class="px-6 py-4">
                {barangayId.status}
              </td>
{/if}
              {#if !$showPrintModel}

                <td class="px-6 py-4">
                  <select
                    class="bg-white"
                    bind:value={barangayId.status}
                    on:change={() =>
                      updateStatus(barangayId.id, barangayId.status)}
                  >
                    <option value="Processing">On Process</option>
                    <option value="Ready for pickup">For Pickup</option>
                    <option value="Claimed">Completed</option>
                  </select>
                </td>
                <td class="px-6 py-4">
                  <div class="flex gap-2">
                    <button
                      class="hover:bg-orange-300 hover:scale-105 px-4 py-2 rounded-full duration-700 hover:text-white"
                      on:click={() => {
                        editValueHandler(i);
                      }}><i class="ri-pencil-line text-base"></i></button
                    >
                    <button
                      class="hover:bg-red-300 hover:scale-105 px-4 py-2 rounded-full duration-700 hover:text-white"
                      on:click={removeData(barangayId.id)}
                      ><i class="ri-delete-bin-line"></i></button
                    >
                    <button
                      class="hover:bg-blue-500 rounded-full px-4 py-2 hover:scale-105 duration-700 text-blue-900 hover:text-white"
                      on:click={() => printPdf(barangayId)}
                      ><i class="ri-printer-line"></i></button
                    >
                  </div>
                </td>
                {/if}
              </tr>

              {#if $compareIDvalue === i}
                <div
                  class="flex flex-col gap-2 p-4 max-w-fit mx-auto rounded-lg mt-2 absolute left-0 right-0 border-2 bg-gray-white z-10"
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
                        TITLE="Height"
                        PLACEHOLDER="Height"
                        bind:this={bgyVarStore.height}
                      />
                    </div>
                    <div class="">
                      <Inputs
                        TITLE="Weight"
                        PLACEHOLDER="Weight"
                        bind:this={bgyVarStore.weight}
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

                    <div class="w-full">
                      <Inputs
                        TITLE="Gender:"
                        TYPE="cordion"
                        bind:this={bgyVarStore.gender}
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
                      PLACEHOLDER="Date Of Appointment"
                      bind:this={bgyVarStore.dateOfAppointment}
                    />
                  </div>

                  <div class="flex gap-2">
                    <button
                      class="bg-orange-300 px-4 py-2 rounded-lg w-1/2"
                      on:click={updateData(barangayId.id)}>Update</button
                    >
                    <button
                      class="bg-red-300 px-4 py-2 rounded-lg w-1/2"
                      on:click={() => {
                        compareIDvalue.set("");
                      }}>Cancel</button
                    >
                  </div>
                </div>
              {/if}
            {/each}
          </tbody>
        </table>
      </div>
    </div>
  </div>
</div>
