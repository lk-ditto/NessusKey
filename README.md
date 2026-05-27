#!/bin/sh

echo "======================================="
echo " DANH SACH VM INVENTORY"
echo "======================================="
vim-cmd vmsvc/getallvms

echo ""
echo "======================================="
echo " VM DANG CHAY (WORLD LIST)"
echo "======================================="
esxcli vm process list

echo ""
echo "======================================="
echo " TIM TOAN BO FILE VMX TRONG DATASTORE"
echo "======================================="

find /vmfs/volumes/ -name "*.vmx"

echo ""
echo "======================================="
echo " KIEM TRA VM KHONG DANG KY"
echo "======================================="

for vmx in $(find /vmfs/volumes/ -name "*.vmx")
do
    VMNAME=$(basename "$vmx")

    vim-cmd vmsvc/getallvms | grep -q "$VMNAME"

    if [ $? -ne 0 ]; then
        echo "[UNREGISTERED] $vmx"
    fi
done

echo ""
echo "======================================="
echo " KIEM TRA FILE LOCK"
echo "======================================="

find /vmfs/volumes/ -name "*.lck"

echo ""
echo "======================================="
echo " KIEM TRA VM INVALID / INACCESSIBLE"
echo "======================================="

vim-cmd vmsvc/getallvms | grep -Ei "invalid|inaccessible"

echo ""
echo "======================================="
echo " HOAN THANH"
echo "======================================="

