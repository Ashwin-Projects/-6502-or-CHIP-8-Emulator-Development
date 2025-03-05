# 6502 Emulator-Development
class Memory:
    def _init_(self, size=65536):
        self.memory = [0] * size
    def read(self, address):
        return self.memory[address]
    def write(self, address, value):
        self.memory[address] = value
class CPU6502:
    def _init_(self):
        self.memory = Memory()
        self.pc = 0x200  # Starting address for programs
        self.sp = 0xFF  # Stack Pointer
        self.a = 0  # Accumulator
        self.x = 0  # X Register
        self.y = 0  # Y Register
        self.status = 0b00000000  # Status register
    def reset(self):
        self.pc = self.memory.read(0xFFFC) | (self.memory.read(0xFFFD) << 8)
    def fetch(self):
        value = self.memory.read(self.pc)
        self.pc += 1
        return value
    def execute_instruction(self):
        opcode = self.fetch()
        if opcode == 0xA9:  # LDA Immediate
            self.a = self.fetch()
        elif opcode == 0x8D:  # STA Absolute
            addr = self.fetch() | (self.fetch() << 8)
            self.memory.write(addr, self.a)
    def run(self):
        while True:
            self.execute_instruction()
cpu = CPU6502()
cpu.reset()
cpu.run()
  
