<script>
  import Button from "../GeneralComponents/Button.svelte";
  import Inputs from "../GeneralComponents/Inputs.svelte";
  import { fly } from "svelte/transition";

  import {
    onSnapsComplaint,
    showComplaintAddModal,
    compareComplaintValue,
    printing,
  } from "../BoundComponents/clickOutside";
  import { formattedDate, showPrintModel } from "./stateStore";

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
  import reportHeader from "./images/header_report.png";

  //handler to show add modal
  const toShowAddModal = () => {
    showComplaintAddModal.set(true);
  };

  //barangayID varStore
  const complaintVarStore = {
    status: "",
    completeName: "",
    complaint: "",
    actionTaken: "",
    location: "",
    date: "",
    kwiri: "",
    trigger: false,
  };

  //submit data to database
  const submitData = async () => {
    const colRef = collection(db, "Complaints");
    await addDoc(colRef, {
      createdAt: serverTimestamp(),
      completeName: complaintVarStore.completeName.BINDTHIS,
      complaint: complaintVarStore.complaint.BINDTHIS,
      actionTaken: complaintVarStore.actionTaken.BINDTHIS,
      location: complaintVarStore.location.BINDTHIS,
      date: complaintVarStore.date.BINDTHIS,
      complaintCounter: increment(1),
    }).then(() => {
      showComplaintAddModal.set(false);
    });
  };

  //fetch data from database
  const colRef = collection(db, "Complaints");
  const q = query(colRef, orderBy("createdAt", "desc"));
  onSnapshot(q, (snapshots) => {
    let fbData = [];
    snapshots.docs.forEach((doc) => {
      let data = { ...doc.data(), id: doc.id };
      console.log(data);
      fbData = [data, ...fbData];
    });
    onSnapsComplaint.set(fbData);
  });

  //removeData from database
  const removeData = async (data) => {
    const docRef = doc(colRef, data);
    await deleteDoc(docRef);
  };

  //updateData from database
  const updateData = async (data) => {
    const docRef = doc(colRef, data);
    setDoc(
      docRef,
      {
        lastUpdated: serverTimestamp(),
        completeName: complaintVarStore.completeName.BINDTHIS,
        completeName: complaintVarStore.completeName.BINDTHIS,
        complaint: complaintVarStore.complaint.BINDTHIS,
        actionTaken: complaintVarStore.actionTaken.BINDTHIS,
        location: complaintVarStore.location.BINDTHIS,
        date: complaintVarStore.date.BINDTHIS,
      },
      { merge: true }
    );
  };

  //showModalComparison
  const editValueHandler = (data) => {
    compareComplaintValue.set(data);
  };

  const detectInputs = () => {
    if (complaintVarStore.kwiri.trim().length < 1) {
      const q = query(colRef, orderBy("createdAt", "desc"));
      onSnapshot(q, (snapshots) => {
        let fbData = [];
        snapshots.docs.forEach((doc) => {
          let data = { ...doc.data(), id: doc.id };
          fbData = [data, ...fbData];
        });
        onSnapsComplaint.set(fbData);
      });
      complaintVarStore.trigger = false;
    } else {
      complaintVarStore.trigger = true;
    }
  };

  const updateStatus = async (userID, selectedStatus) => {
    const docRef = doc(colRef, userID);
    const updatedData = {
      status: selectedStatus,
      lastUpdated: serverTimestamp(),
    };
    await setDoc(docRef, updatedData, { merge: true });
  };

  const handlerSearch = () => {
    if (complaintVarStore.trigger) {
      const q = query(
        colRef,
        orderBy("createdAt", "desc"),
        where("completeName", "==", complaintVarStore.kwiri)
      );
      onSnapshot(q, (snapshots) => {
        let fbData = [];
        snapshots.docs.forEach((doc) => {
          let data = { ...doc.data(), id: doc.id };
          fbData = [data, ...fbData];
        });
        onSnapsComplaint.set(fbData);
      });
      complaintVarStore.trigger = false;
    }
  };

  // let printing = false;
</script>

<div class="m-2 mx-auto text-xs">
  <div class="min-h-[50vh] p-10">
    <div class="flex gap-2 items-center mb-2">
      <div class="w-full flex gap-2">
        <div class="">
          <Button TITLE="New Blotter" on:click={toShowAddModal} />
        </div>

        <div class="">
          <Button TITLE="Print" on:click={() => showPrintModel.set(true)} />
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
          bind:value={complaintVarStore.kwiri}
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
                      setTimeout(() => print(), 100)
                      setTimeout(() => {
                        $printing = false;
                        $showPrintModel = false;
                      }, 2000)

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

    {#if $showComplaintAddModal}
      <div
        class="flex flex-col gap-2 bg-white p-4 max-w-fit mx-auto rounded-lg mt-2 absolute left-0 right-0 border-2 border-guiColor z-10"
      >
        <p class="text-xl text-center font-bold p-2 text-slate-500">
          New Blotter
        </p>
        <div class="">
          <Inputs
            TITLE="Complainant Name:"
            PLACEHOLDER="Complainant Name"
            bind:this={complaintVarStore.completeName}
          />
        </div>

        <div class="flex gap-2">
          <div class="w-full">
            <Inputs
              TITLE="Date:"
              PLACEHOLDER="Date"
              TYPE="date"
              bind:this={complaintVarStore.date}
            />
          </div>

          <div class="w-full">
            <Inputs
              TITLE="Location:"
              PLACEHOLDER="Location of Incidence"
              bind:this={complaintVarStore.location}
            />
          </div>
        </div>

        <div class="">
          <Inputs
            TITLE="Complaint:"
            PLACEHOLDER="Complaint"
            bind:this={complaintVarStore.complaint}
          />
        </div>

        <div class="">
          <Inputs
            TITLE="Action taken:"
            PLACEHOLDER="Action taken"
            bind:this={complaintVarStore.actionTaken}
          />
        </div>

        <div class="flex gap-2">
          <Button TITLE="Submit" on:click={submitData} />
          <Button
            TITLE="Cancel"
            on:click={() => {
              showComplaintAddModal.set(false);
            }}
          />
        </div>
      </div>
    {/if}
    <div class="" in:fly={{ x: 400, duration: 1000 }}>
      <div class="relative ">
        {#if $showPrintModel}
        <img src={reportHeader} alt="" style="margin-top: -130px; margin-bottom: 100px" />
      {/if}
        <table class="w-full text-sm text-left text-gray-500">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50">
            <tr>
              <th scope="col" class="px-6 py-3"> Date of Appointment</th>
              <th scope="col" class="px-6 py-3"> Firstname </th>
              <th scope="col" class="px-6 py-3"> MI</th>
              <th scope="col" class="px-6 py-3"> Lastname </th>
              <th scope="col" class="px-6 py-3"> Suffix </th>
              <th scope="col" class="px-6 py-3"> Contact# </th>
              <th scope="col" class="px-6 py-3"> Address </th>
              {#if !$showPrintModel}
                <th scope="col" class="px-6 py-3"> evidence </th>
              {/if}
              <th scope="col" class="px-6 py-3"> complaint </th>
              <th scope="col" class="px-6 py-3"> location</th>
              {#if !$showPrintModel}
                <th scope="col" class="px-6 py-3"> Action Taken </th>
                <th scope="col" class="px-6 py-3"> Action</th>
              {/if}
              {#if $showPrintModel}
                <th scope="col" class="px-6 py-3"> Status</th>
              {/if}
            </tr>
          </thead>
          <tbody>
            {#each $onSnapsComplaint as complaintData, i}
              {#if complaintData.defendantName == ""}
                <tr class="bg-white border-b">
                  <td class="px-6 py-4"> {complaintData.dateOfAppointment} </td>
                  <th
                    scope="row"
                    class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap"
                  >
                    {complaintData.firstName}
                  </th>
                  <td class="px-6 py-4"> {complaintData.middleInitial} </td>
                  <td class="px-6 py-4"> {complaintData.lastName} </td>
                  <td class="px-6 py-4"> {complaintData.suffixName} </td>
                  <td class="px-6 py-4"> {complaintData.contactNumber} </td>
                  <td class="px-6 py-4"> {complaintData.address} </td>
                  {#if !$showPrintModel}
                    <td class="px-6 py-4">
                      <a href={complaintData.evidenceURL}>
                        <img
                          src={complaintData.evidenceURL}
                          alt="ID Picture"
                          class="h-10 w-10"
                        />
                      </a>
                    </td>
                  {/if}
                  <td class="px-6 py-4"> {complaintData.complaint} </td>

                  <td class="px-6 py-4"> {complaintData.location}</td>
                  {#if $showPrintModel}
                    <td class="px-6 py-4">
                      {complaintData.status ?? "Pending"}</td
                    >
                  {/if}
                  {#if !$showPrintModel}
                    <td class="px-6 py-4">
                      <select
                        class="bg-white"
                        bind:value={complaintData.status}
                        on:change={() =>
                          updateStatus(complaintData.id, complaintData.status)}
                      >
                        <option value="Processing">On Process</option>
                        <option value="ProcBrgy">Proceed To Barangay</option>
                        <option value="Completed">Completed</option>
                      </select>
                    </td>
                    <td>
                      <div class="flex gap-2">
                        <button
                          class="hover:bg-orange-300 px-4 py-2 rounded-full duration-700 hover:scale-105"
                          on:click={() => {
                            editValueHandler(i);
                          }}><i class="ri-pencil-line"></i></button
                        >

                        <button
                          class="hover:bg-red-800 px-4 py-2 rounded-full duration-700 hover:scale-105 hover:text-white"
                          on:click={removeData(complaintData.id)}
                          ><i class="ri-delete-bin-line"></i></button
                        >
                      </div>
                    </td>
                  {/if}
                </tr>
              {/if}
            {/each}
          </tbody>
        </table>
        <div style="height: 100px;"></div>
        <div
          style="height: 1px; width: 100%; border: 1px solid lightgray"
        ></div>
        <div style="height: 100px;"></div>

        <table class="w-full text-sm text-left text-gray-500">
          <thead class="text-xs text-gray-700 uppercase bg-gray-50">
            <tr>
              <th scope="col" class="px-6 py-3"> Date of Appointment</th>
              <th scope="col" class="px-6 py-3"> Firstname </th>
              <th scope="col" class="px-6 py-3"> MI</th>
              <th scope="col" class="px-6 py-3"> Lastname </th>
              <th scope="col" class="px-6 py-3"> Suffix </th>
              <th scope="col" class="px-6 py-3"> Contact# </th>
              <th scope="col" class="px-6 py-3"> Address </th>
              <th scope="col" class="px-6 py-3"> Defendant name </th>
              <th scope="col" class="px-6 py-3"> defendant location </th>
              {#if !$showPrintModel}
                <th scope="col" class="px-6 py-3"> evidence </th>
              {/if}
              <th scope="col" class="px-6 py-3"> complaint </th>
              <th scope="col" class="px-6 py-3"> location</th>
              {#if !$showPrintModel}
                <th scope="col" class="px-6 py-3"> Action Taken </th>
                <th scope="col" class="px-6 py-3"> Action</th>
              {/if}
              {#if $showPrintModel}
                <th scope="col" class="px-6 py-3"> Status</th>
              {/if}
            </tr>
          </thead>
          <tbody>
            {#each $onSnapsComplaint as complaintData, i}
              {#if complaintData.defendantName != ""}
                <tr class="bg-white border-b">
                  <td class="px-6 py-4"> {complaintData.dateOfAppointment} </td>
                  <th
                    scope="row"
                    class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap"
                  >
                    {complaintData.firstName}
                  </th>
                  <td class="px-6 py-4"> {complaintData.middleInitial} </td>
                  <td class="px-6 py-4"> {complaintData.lastName} </td>
                  <td class="px-6 py-4"> {complaintData.suffixName} </td>
                  <td class="px-6 py-4"> {complaintData.contactNumber} </td>
                  <td class="px-6 py-4"> {complaintData.address} </td>
                  <td class="px-6 py-4"> {complaintData.defendantName} </td>
                  <td class="px-6 py-4"> {complaintData.defendantLocation} </td>
                  {#if !$showPrintModel}
                    <td class="px-6 py-4">
                      <a href={complaintData.evidenceURL}>
                        <img
                          src={complaintData.evidenceURL}
                          alt="ID Picture"
                          class="h-10 w-10"
                        />
                      </a>
                    </td>
                  {/if}

                  <td class="px-6 py-4"> {complaintData.complaint} </td>

                  <td class="px-6 py-4"> {complaintData.location}</td>
                  {#if !$showPrintModel}
                    <td class="px-6 py-4">
                      <select
                        class="bg-white"
                        bind:value={complaintData.status}
                        on:change={() =>
                          updateStatus(complaintData.id, complaintData.status)}
                      >
                        <option value="Processing">On Process</option>
                        <option value="ProcBrgy">Proceed To Barangay</option>
                        <option value="Completed">Completed</option>
                      </select>
                    </td>
                    <td>
                      <div class="flex gap-2">
                        <button
                          class="hover:bg-orange-300 px-4 py-2 rounded-full duration-700 hover:scale-105"
                          on:click={() => {
                            editValueHandler(i);
                          }}><i class="ri-pencil-line"></i></button
                        >

                        <button
                          class="hover:bg-red-800 px-4 py-2 rounded-full duration-700 hover:scale-105 hover:text-white"
                          on:click={removeData(complaintData.id)}
                          ><i class="ri-delete-bin-line"></i></button
                        >
                      </div>
                    </td>
                  {/if}
                  {#if $showPrintModel}
                    <td class="px-6 py-4">
                      {complaintData.status ?? "Pending"}
                    </td>
                  {/if}
                </tr>
              {/if}
            {/each}
          </tbody>
        </table>
      </div>

      <!-- {#each $onSnapsComplaint as value, i}
            <div class="flex justify-center items-center " in:fly={{x:400, duration:1000}}>
                <p class="w-[10%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Date</p>
                <p class="w-[23%] border-2 border-white bg-slate-100 p-2">{value.date}</p>

                <p class="w-[25%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Complete Name</p>
                <p class="w-[50%] border-2 border-white bg-slate-100 p-2">{value.completeName}</p>

                <p class="w-[10%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Complaint</p>
                <p class="w-full border-2 border-white bg-slate-100 p-2">{value.complaint}</p>

                <p class="w-[10%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Action</p>
                <p class="w-full border-2 border-white bg-slate-100 p-2">{value.actionTaken}</p>

                <p class="w-[10%] font-bold border-b-2 border-white bg-slate-300 p-2 ">Location</p>
                <p class="w-[60%] border-2 border-white bg-slate-100 p-2">{value.location}</p>
                
                <div class="flex w-[30%] bg-slate-100">
                    <button class="bg-red-500 font-bold text-white w-full p-2 hover:bg-red-600 border-b-2 border-white"
                    on:click={removeData(value.id)}
                    >Delete</button>

                    <button class="bg-blue-500 font-bold text-white w-full p-2 hover:bg-blue-600 border-b-2 border-white"
                    on:click={()=>{editValueHandler(i)}}
                    >Edit</button>
                </div>


                {#if $compareComplaintValue === i}
                    <div class="">
                        <div class="flex flex-col gap-2 bg-guiColor p-4 max-w-fit mx-auto rounded-lg mt-2 absolute left-0 right-0 border-2 border-slate-200 z-10">
                            <p class="text-xl text-center font-bold p-2 text-slate-500">Modify Values</p>
                            <div class="">
                                <Inputs TITLE="Complete Name:" PLACEHOLDER="Complete Name" bind:this={complaintVarStore.completeName}/>
                            </div>
                
                            <div class="flex justify-center gap-2">
                                <div class="w-full">
                                    <Inputs TITLE="Date:" PLACEHOLDER="Date" TYPE="date" bind:this={complaintVarStore.date}/>
                                </div>
                                <div class="w-full">
                                    <Inputs TITLE="Locaton:" PLACEHOLDER="Locaton Of Incident" bind:this={complaintVarStore.location}/>
                                </div>
                            </div>
                
                            <div class="flex justify-center gap-2">
                                <div class="w-full">
                                    <Inputs TITLE="Complaint:" PLACEHOLDER="Complaint" bind:this={complaintVarStore.complaint}/>
                                </div>
                            
                            </div>
                
                            <div class="">
                                <Inputs TITLE="Action taken:" PLACEHOLDER="Action taken" bind:this={complaintVarStore.actionTaken}/>
                            </div>
                            
                            <div class="flex gap-2">
                                <Button TITLE="Confirm Edit" on:click={updateData(value.id)}/>
                                <Button TITLE="Cancel" on:click={() => {compareComplaintValue.set("")}}/>
                            </div>

                            
                        </div>
                    </div>
                {/if}
            </div>
            {/each} -->
    </div>
  </div>
</div>
