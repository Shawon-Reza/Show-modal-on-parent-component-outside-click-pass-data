```jsx
import React, { useEffect, useRef } from "react";

const Modal = ({ isOpen, onClose, children }) => {
  const modalRef = useRef(null);  // Reference to the modal container
  const modalContentRef = useRef(null);  // Reference to modal content to prevent closing when clicked inside

  // Close the modal if clicking outside
  useEffect(() => {
    const handleClickOutside = (event) => {
      if (
        modalRef.current &&
        !modalRef.current.contains(event.target) && // Clicked outside modal container
        modalContentRef.current &&
        !modalContentRef.current.contains(event.target) // Clicked outside modal content
      ) {
        onClose(); // Close modal if clicked outside
      }
    };

    if (isOpen) {
      document.addEventListener("mousedown", handleClickOutside); // Add event listener when modal is open
    }
    return () => {
      document.removeEventListener("mousedown", handleClickOutside); // Clean up when modal is closed
    };
  }, [isOpen, onClose]);

  // Don't render the modal if it's not open
  if (!isOpen) return null;

  return (
    <div className="fixed inset-0 bg-black/30 flex items-center justify-center z-50">
      <div ref={modalRef} className="bg-white p-6 rounded-lg w-1/3">
        <div ref={modalContentRef} className="space-y-4">
          {/* Modal Content */}
          {children}
        </div>

        {/* Close button */}
        <button onClick={onClose} className="mt-4 bg-gray-300 py-2 px-4 rounded">
          Close
        </button>
      </div>
    </div>
  );
};

export default Modal;

```

```jsx
import React, { useState } from "react";
import Modal from "./Modal";

const ParentComponent = () => {
  const [isModalOpen, setIsModalOpen] = useState(false);

  const handleOpenModal = () => setIsModalOpen(true);
  const handleCloseModal = () => setIsModalOpen(false);

  return (
    <div>
      <button onClick={handleOpenModal}>Open Modal</button>

      {/* Modal Component */}
      <Modal isOpen={isModalOpen} onClose={handleCloseModal}>
        <h2>Modal Content</h2>
        <p>This is a reusable modal component.</p>
        {/* Add other content or form elements as needed */}
      </Modal>
    </div>
  );
};

export default ParentComponent;
